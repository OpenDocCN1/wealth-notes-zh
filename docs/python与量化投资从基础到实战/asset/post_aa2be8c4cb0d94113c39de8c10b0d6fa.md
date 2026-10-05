
futures_account.order(context.symbol1, -3, 'open')
futures_account.order(context.symbol2, -1, 'open')

# 平仓
if close0 - 1.6 * close1 - 0.5 * close2 - 1100 < 100:
    if short_position_0 != 0:
        # print futures_account.current_date, '平仓'
        # print close0 - 1.6 * close1 - 0.5 * close2 - 1100
        futures_account.order(context.symbol0,short_position_0,
'close')
    if long_position_1 != 0:
        futures_account.order(context.symbol1,-long_position_1,
'close')
    if long_position_2 != 0:
        futures_account.order(context.symbol2,-long_position_2,
'close')
if close0 - 1.6 * close1 - 0.5 * close2 - 1100 > 100:
    if long_position_0 != 0:
        # print futures_account.current_date, '平仓'
        # print close0 - 1.6 * close1 - 0.5 * close2 - 1100
        futures_account.order(context.symbol0,short_position_0,
'close')
    if short_position_1 != 0:
        futures_account.order(context.symbol1,-long_position_1,
'close')
    if short_position_2 != 0:
        futures_account.order(context.symbol2,-long_position_2,
'close')

回测结果如图 6-11 所示。

该策略还有以下特点。

+   （1）虚拟钢厂套利策略是基于基本面逻辑的套利策略，如果基本面发生重大变化，则应及时止损止盈。

+   （2）可以看到，策略的本质其实是一个固定了上下轨道的突破策略，其上下轨的确定是根据模拟钢厂生产的利润传导机制确定的。

+   （3）在当前低迷的市场情况下，钢企合理利用期货市场进行套期保值、管控风险，具有现实意义。

# （4）交易次数较少。

同样，该策略在 2017 年遭遇了一定的回撤，在国内商品市场的四大板块中，贵金属、农产品、工业品均维持了近一年的无趋势行情，有色金属也仅有微弱的趋势。另一方面，我们的定价方程是基于基本面数据的，随着时间的推移，基本面数据发生改变，定价方程也应该随着改变，否则会造成大幅回撤。

![](img/0b89c6ed12810914fc2fb80a08af1fae_265_0.png)

# 图 6-11

# 2. 跨期套利

与跨品种套利不同，跨期套利是在同一市场、同一品种、不同的到期月份的合约之间进行套利，其中涉及非主力合约的问题。非主力合约的成交量比较小，有的甚至低到几十元数百手，很容易被几手交易量将价格拉至异常。这种异常实际上较难捕捉，只会放大回测结果。我们以沪镍为例，其主力合约主要在 1、5、9 月交割，其余月份的成交量很低。而 1 月交割合约的活跃期为 7 月至 12 月，5 月交割合约的活跃期为 11 月至来年 4 月，所以在构建 1 月交割合约和 5 月交割合约的套利组合时，我们选择 11、12 月作为套利策略实施的时间进行测试，在 12 月底如果有持仓，则强行平仓。在这个时间段内，1 月交割合约为主力合约，5 月交割合约为次主力合约。

代码如下：

```
from collections import deque
import numpy as np
import talib as ta
import matplotlib.pyplot as plt
from CAL.PyCAL import *
NI1601 = DataAPI.MktFutmGet(ticker=u"NI1601",beginDate=u"",endDate=u"",field=u"endDate,turnoverVol",pandas="1").sort_values(by='endDate')
NI1701 = DataAPI.MktFutmGet(ticker=u"NI1701",beginDate=u"",endDate=u"",field=u"endDate,turnoverVol",pandas="1").sort_values(by='endDate')
NI = NI1601.append(NI1701)
NI.drop_duplicates(subset='endDate', keep='first', inplace=True)
NI = NI.set_index('endDate')
fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
ax.set_axisbelow('True')
ax = NI.plot(kind='bar', ax=ax, color='r', legend=False)
s = ax.set_title(u'沪镍1月交割月成交量', fontproperties=font, fontsize=16)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_266_0.png)

在套利时，我们需要计算均衡价差，一般有两种算法。

+   ○ 模拟交割法：通过模拟交割过程中的各项费用，来计算套利过程中的各项成本，从而得出均衡价差。优点为容易计算，价差变化小；缺点为在实际情况下价差很

少恢复到这一水平。

+   历史滞后均值法：通过历史的价差选择时间窗口长度，得到均衡价差。优点为容易计算，价差是动态的；缺点为不能保证样本外价差恢复到这一水平。

我们以选定套利组合的开盘价差为对象，使用历史滞后均值法，取 N 日价差均值为中线，取中线加上 N 日价差的 M 倍均值为上轨，取中线减去 N 日价差的 M 倍均值为下轨来构造布林带通道，当价差突破上轨时，做空价差。我们选择 1、5、9 这三个月份交割合约，滚动地进行测试，其中 1 月至 5 月合约测试的时间为 11 月和 12 月，5 月至 9 月合约测试的时间为 3 月和 4 月，9 月至来年 1 月合约测试的时间为 7 月和 8 月，所以全年有半年不进行操作。

相关参数及设定如下。

+   成交价设置为当日开盘价。这里考虑到次主力合约的成交量可能较少，所以设置为 3 个滑点。

+   开仓信号为价差超过上轨或者突破下轨，信号触发当天就按所设定的成交价进行成交。如果平仓信号达到中线附近或者所设定的套利期限结束，则强行平仓。

+   手续费设为 6 元一手。

+   回测时间为 2015 年 5 月至 2017 年 12 月。

+   暂不考虑止损。

策略代码如下：

```
universe = ['NIMO']            # 策略期货合约
start = '2015-05-18'           # 回测开始时间
end = '2017-12-31'             # 回测结束时间
capital_base = 1000000         # 初试可用资金
refresh_rate = 1               # 调仓周期
freq = 'd'                     # 调仓频率: m-> 分钟; d-> 日
margin_rate = 0.09
commission = {'NI': (6, 'perShare')}
slippage = Slippage(3, 'perShare')
accounts = {
    'futures_account':AccountConfig(account_type='futures',capital_base=capital_base,margin_rate=margin_rate,commission=commission,slippage=slippage
)
}
months = ['07', '08', '11', '12', '03', '04']

mid, up, low = [], [], []
def initialize(context):      # 初始化虚拟期货账户，一般用于设置计数器、回测辅助变量等
    context.mean = deque([], maxlen=10)
    context.std = deque([], maxlen=10)
    context.M0 = ''
    context.M1 = ''
def handle_data(context):      # 回测调仓逻辑，在每个调仓周期运行一次，可在此函数内实现信号生产、生成调仓指令
    futures_account = context.get_account('futures_account')
    today = str(context.current_date)
    if today[5:7] in ['11', '12']:
        M0 = universe[0][:2] + str(int(today[2:4])+1) + '01'
        M1 = universe[0][:2] + str(int(M0[2:4])+(int(M0[4:])+4)/12)+'0'+str((int(M0[4:])+4)%12)
    elif today[5:7] in ['07', '08']:
        M0 = universe[0][:2] + str(int(today[2:4])) + '09'
        M1 = universe[0][:2] + str(int(M0[2:4])+(int(M0[4:])+4)/12)+'0'+str((int(M0[4:])+4)%12)
    elif today[5:7] in ['03', '04']:
        M0 = universe[0][:2] + str(int(today[2:4])) + '05'
        M1 = universe[0][:2] + str(int(M0[2:4])+(int(M0[4:])+4)/12)+'0'+str((int(M0[4:])+4)%12)

    long_position = futures_account.get_positions().get(context.M0, dict()).get('long_amount', 0)
    short_position = futures_account.get_positions().get(context.M1, dict()).get('short_amount', 0)

    if today[5:7] not in months:
        if long_position != 0:
            futures_account.order(context.M0, -20, 'close')
            futures_account.order(context.M1, 20, 'close')
        return

    M0_open_price = context.history(symbol=M0, attribute='openPrice', time_range=11)[M0]['openPrice']
```
# Python 与量化投资：从基础到实战
```
M1_open_price = context.history(symbol=M1, attribute='openPrice', time_range=11)[M1]['openPrice']

diff = np.array(M0_open_price - M1_open_price, dtype=float)
context.mean.append(ta.MA(diff, 10)[-1])
context.std.append(ta.STDDEV(diff, 10)[-1])

if diff[-1] < context.mean[-1] - 1 * context.std[-1] and long_position == 0:
    futures_account.order(M1, -20, 'open')
    futures_account.order(M0, 20, 'open')
if diff[-1] > context.mean[-1] - 0.2 * context.std[-1] and long_position != 0:
    futures_account.order(M1, 20, 'close')
    futures_account.order(M0, -20, 'close')

context.M0 = M0
context.M1 = M1
```

回测结果如图 6-12 所示。

![](img/0b89c6ed12810914fc2fb80a08af1fae_269_0.png)

图 6-12

此处仅为一个简单的示例，通过回测发现，本策略并没有取得什么收益。一般来说，策略亏损的情况发生在如下两种情况下。

+   ○ 价差形成了长期趋势。虽然这时价差恢复到了 $N$ 日平均线，但是与开仓点相比，其偏离程度很大。

○ 价差偏差程度较小，这时均值回复的收益不足以弥补手续费和冲击成本造成的损失。

# 6.5.3 CTA 策略表现分析

在前两节讲到了基本的两类 CTA 策略：趋势跟随策略和均值回复策略。那么，如何评价这些策略的表现呢？这里可以运用优矿专业版提供的组合分析工具进行策略分析。

以简单的双均线突破策略为例，策略代码如下：

```
import talib
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

### 参数初始化
universe = ['RB1610']                # 策略证券池
start = pd.datetime(2016, 6, 1)      # 回测开始时间
end   = pd.datetime(2016, 9, 1)      # 回测结束时间
capital_base = 1e4                   # 初试可用资金
refresh_rate = 1                     # 调仓周期
freq = 'd'                           # 调仓频率: s-> 秒; m-> 分钟; d-> 日;

## 自动生成保证金比例： margin_rate
margin_ratio=DataAPI.FutuGet(ticker=universe,field=['ticker','tradeMarginRatio'], pandas = '1')
margin_rate = dict(zip(margin_ratio.ticker.tolist(), [0.01*index for index in margin_ratio.tradeMarginRatio.tolist()]))
accounts = {
    'futures_account': AccountConfig(account_type='futures', capital_base=capital_base, margin_rate=margin_rate)
}

### 策略初始化函数，一般用于设置计数器、回测辅助变量等
def initialize(context):
    pass
```
# Python 与量化投资：从基础到实战
```
### 回测调仓逻辑，在每个调仓周期运行一次，可在此函数内实现信号生产、生成调仓指令
def handle_data(context):
    futures_account = context.get_account('futures_account')
    symbol, amount = universe[0], 1
    history_data = context.history(symbol = symbol, time_range = 20)[symbol]
    MA_S = talib.MA(history_data['closePrice'].apply(float).values, timeperiod=5)
    MA_L = talib.MA(history_data['closePrice'].apply(float).values, timeperiod=10)
    current_long = futures_account.get_positions().get(symbol, dict()).get('long_amount', 0)
    current_short = futures_account.get_positions().get(symbol, dict()).get('short_amount', 0)
    if MA_S[-1] > MA_L[-1] and MA_S[-2] < MA_L[-2]:
        if current_short > 0:
            # print futures_account.current_date, futures_account.current_time, '买入平仓'
            futures_account.order(symbol, current_short, 'close')
        if current_long < amount:
            # print futures_account.current_date, futures_account.current_time, '买入开仓'
            futures_account.order(symbol, amount, 'open')
    if MA_S[-1] < MA_L[-1] and MA_S[-2] > MA_L[-2]:
        if current_long > 0:
            # print futures_account.current_date, futures_account.current_time, '卖出平仓'
            futures_account.order(symbol, -current_long, 'close')
        if current_short < amount:
            # print futures_account.current_date, futures_account.current_time, '卖出开仓'
            futures_account.order(symbol, -amount, 'open')
```

回测结果如图 6-13 所示。
