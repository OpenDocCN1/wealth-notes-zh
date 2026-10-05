
```
['ticker','tradeMarginRatio'], pandas = '1')
    margin_rate = dict(zip(margin_ratio.ticker.tolist(), [0.01*index for index in margin_ratio.tradeMarginRatio.tolist()]))
    accounts = {
        'futures_account': AccountConfig(account_type='futures',
                                   capital_base=capital_base,
        margin_rate=margin_rate)
    }

    ### 策略初始化函数，一般用于设置计数器、回测辅助变量等
    def initialize(context):
        context.high_point = None
        context.low_point = None
        pass

    ### 回测调仓逻辑；在每个调仓周期运行一次，可在此函数内实现信号生产、生成调仓指令
    def handle_data(context):
        futures_account = context.get_account('futures_account')
        symbol, amount = universe[0], 1
        history_data = context.history(symbol = symbol, attribute=['highPrice', 'lowPrice', 'closePrice'], time_range = 5)[symbol]
        pre_close_price = history_data['closePrice'].tolist()[-1]
        if history_data.highPrice.tolist().index(history_data.highPrice.max()) == int(len(history_data)/2):
            context.high_point = history_data.highPrice.max()
        if history_data.lowPrice.tolist().index(history_data.lowPrice.min()) == int(len(history_data)/2):
            context.low_point = history_data.highPrice.min()

        current_long = futures_account.get_positions().get(symbol, dict()).get('long_amount', 0)
        current_short = futures_account.get_positions().get(symbol, dict()).get('short_amount', 0)
        if pre_close_price > context.high_point:
            if current_short > 0:
                print context.current_date, '买入平仓'
                futures_account.order(symbol, current_short, 'close')
            if current_long < amount:
                print context.current_date, '买入开仓'
                futures_account.order(symbol, amount, 'open')

if pre_close_price < context.low_point:
    if current_long > 0:
        print context.current_date, '卖出平仓'
        futures_account.order(symbol, -current_long, 'close')
    if current_short < amount:
        print context.current_date, '卖出开仓'
        futures_account.order(symbol, -amount, 'open')

回测结果如图 6-9 所示。

![](img/0b89c6ed12810914fc2fb80a08af1fae_252_0.png)

图 6-9

从结果上来看，该策略对标的趋势的抓取较为精准，但是收益的波动率过大，有一定的改善空间。

本节之前所阐述的策略都是日频 CTA 策略，接下来我们看看日内 CTA 策略。

R-Breaker 是一种短线日内交易策略，结合了趋势和反转这两种交易方式。该策略长期被 Future Thruth 杂志评为最赚钱的策略之一，尤其在标普 500 股指期货上效果最佳。该策略的主要特点如下。

-   (1) 根据前一个交易日的收盘价、最高价和最低价数据，通过一定方式计算出 6 个价位，从大到小依次为突破买入价、观察卖出价、反转卖出价、反转买入价、观察买入价和突破卖出价，以此来形成在当前交易日盘中交易的触发条件。通过对计算方式的调整，可

以调节 6 个价格间的距离，进一步改变触发条件。

-   (2) 根据盘中价格走势，实时判断触发条件，具体条件如下。

    -   ◎ 在日内最高价超过观察卖出价后，盘中价格出现回落且进一步跌破反转卖出价构成的支撑线时，采取反转策略，即在该点位（反手、开仓）做空。

    -   ◎ 在日内最低价低于观察买入价后，盘中价格出现反弹且进一步超过反转买入价构成的阻力线时，采取反转策略，即在该点位（反手、开仓）做多。

    -   ◎ 在空仓的情况下，如果盘中价格超过突破买入价，则采取趋势策略，即在该点位开仓做多。

    -   ◎ 在空仓的情况下，如果盘中价格跌破突破卖出价，则采取趋势策略，即在该点位开仓做空。

-   (3) 设定止损及止盈条件。

-   (4) 设定过滤条件。

-   (5) 在每日收盘前对所持合约进行平仓。

具体来看，这 6 个价位形成的阻力和支撑位的计算过程如下。

观察卖出价 = High + 0.35 × (Close − Low)

观察买入价 = Low − 0.35 × (High − Close)

反转卖出价 = 1.07 / 2 × (High + Low) − 0.07 × Low

反转买入价 = 1.07 / 2 × (High + Low) − 0.07 × High

突破买入价 = 观察卖出价 + 0.25 × (观察卖出价 − 观察买入价)

突破卖出价 = 观察买入价 − 0.25 × (观察卖出价 − 观察买入价)

其中，High、Close 和 Low 分别为昨日最高价、昨日收盘价和昨日最低价。这 6 个价位从大到小依次是：突破买入价、观察卖出价、反转卖出价、反转买入价、观察买入价和突破卖出价。

策略代码如下。首先，引入相关的库：

```
import DataAPI
import numpy as np
```

```
import pandas as pd
import talib as ta
from collections import deque
from itertools import product
import datetime
import matplotlib
import matplotlib.pyplot as plt
import matplotlib.cm as cm
import seaborn as sns
from CAL import *
cal = Calendar('China.SSE')

策略相关的代码如下：

### 策略初始化函数
universe = ['RB M0']                    # 策略交易的期货合约，此处选择IH1609
start = "2017-01-02"                   # 回测开始时间
end   = "2017-12-31"                   # 回测结束时间
capital_base = 1e6                      # 初始可用资金
refresh_rate = (1, 5)                   # 算法调用周期
freq = 'm'                              # 算法调用频率: m-> 分钟; d-> 日;
commission = {'RB': (0.000025, 'perValue')}
slippage = Slippage(0, 'perValue')
amount = 20
accounts = {
    'futures_account': AccountConfig(account_type='futures',
capital_base=capital_base, commission=commission, slippage=slippage)
}
def initialize(context):
    context.pipe_length = 2
    context.refresh_rate = refresh_rate[1]
    context.keys = [u'openPrice', u'highPrice', u'lowPrice', u'closePrice',
 u'turnoverVol', u'tradeDate']
    context.data = {key:deque([], maxlen=context.pipe_length) for key in context.keys}
    context.high = np.NAN
    context.low = np.NAN
    context.close = np.NAN
    context.count1 = 0
    context.count2 = 0
def handle_data(context):
    futures_account = context.get_account('futures_account')
```

# Python 与量化投资：从基础到实战

```
symbol = context.get_symbol(universe[0])
    long_position = futures_account.get_positions().get(symbol,
dict()).get('long_amount', 0)
    short_position = futures_account.get_positions().get(symbol,
dict()).get('short_amount', 0)
    if context.current_minute == '09:30':
        yester_data = DataAPI.MktFutdGet(tradeDate=context.previous_date,
ticker=symbol, field=[u'closePrice', u'highestPrice',u'lowestPrice'],
pandas="1")
        context.high = yester_data['highestPrice'].iat[0]
        context.low = yester_data['lowestPrice'].iat[0]
        context.close = yester_data['closePrice'].iat[0]
    if context.current_minute > '09:30' and context.current_minute < '14:55':
        # =================================
        if len(context.data['openPrice']) < context.pipe_length:
            hist = context.history(symbol=symbol, attribute=context.keys[:-1], time_range=context.refresh_rate*context.pipe_length, freq='1m')[symbol]
        else:
            hist = context.history(symbol=symbol, attribute=context.keys[:-1], time_range=context.refresh_rate, freq='1m')[symbol]
        current_data = {key:[] for key in context.keys}
        for i in range(len(hist)/context.refresh_rate):
            current_bar = hist[context.refresh_rate*i:context.refresh_rate*(i+1)]
            current_bar['tradeDate'] = [i[:10] for i in current_bar.index]
            current_data['closePrice'].append(current_bar.ix[len(current_bar)-1, 'closePrice'])
            current_data['openPrice'].append(current_bar.ix[0, 'openPrice'])
            current_data['highPrice'].append(current_bar['highPrice'].max())
            current_data['lowPrice'].append(current_bar['lowPrice'].min())
            current_data['turnoverVol'].append(current_bar['turnoverVol'].sum())
            current_data['tradeDate'].append(current_bar.ix[len(current_bar)-1, 'tradeDate'])
        for i in context.keys:
            for j in current_data[i]:
                context.data[i].append(j)
```

```
# ==============================
data = context.data
before_2_close = data['closePrice'][-2]
before_2_high = data['highPrice'][-2]
before_1_close = data['closePrice'][-1]
before_1_high = data['highPrice'][-1]
before_1_low = data['lowPrice'][-1]
before_1_open = data['openPrice'][-1]
# 观察卖出价
ssetup=context.high+0.35*(context.close-context.low)
# 观察买入价
bsetup=context.low-0.35*(context.high-context.close)
# 反转卖出价
senter=(1+0.07)/2*(context.high+context.low)-0.07*context.low
# 反转买入价
benter = (1+0.07)/2*(context.high+context.low)-0.07*context.high
# 突破买入价
bbreak = ssetup+0.25*(ssetup-bsetup)
# 突破卖出价
sbreak = bsetup-0.25*(ssetup-bsetup)

## 趋势
if before_2_close <= bbreak and before_1_close > bbreak:
    if long_position == 0:
        futures_account.order(symbol, amount, 'open')
    if short_position != 0:
        futures_account.order(symbol, short_position, 'close')
if before_2_close >= sbreak and before_1_close < sbreak:
    if short_position == 0:
        futures_account.order(symbol, -amount, 'open')
    if long_position != 0:
        futures_account.order(symbol, -long_position, 'close')
## 反转
### 多单反转
if before_1_high > ssetup and before_1_close > senter:
    context.count1 = 1
if context.count1 == 1 and before_1_close < senter:
    if long_position > 0:
        futures_account.order(symbol, ~long_position, 'close')
        futures_account.order(symbol, -amount, 'open')
### 空单反转
```
