
```
cross[sec] = 0
return pd.Series(cross)

# 定义并注册 Signal 模块
def initialize(context):
    up = Signal('BollUp')
    down = Signal('BollDown')
    cross = Signal('CRX', cross_situation)
    context.signal_generator = SignalGenerator(up, down, cross)

def handle_data(context):
    current_universe = context.get_universe(exclude_halt=True)
    boll_up, boll_down, crx = context.signal_result['BollUp'], context.signal_result['BollDown'], context.signal_result['CRX']

    buylist = []
    account = context.get_account('fantasy_account')
    cash = account.cash
    current_positions = account.get_positions()
    for stock in current_universe:
        # 收盘价上穿 BOLL 上轨，且无持仓
        if crx[stock] == 1 and stock not in current_positions:
            buylist.append(stock)

        # 收盘价下穿 BOLL 中轨，且有持仓
        elif crx[stock] == -1 and stock in current_positions:
            order_to(stock, 0)  # 全部卖出
            cash += current_positions[stock].amount * context.current_price(stock)  # 估计买入金额

    if len(buylist)==0:
        weight = 0
    else:
        weight = min(1.0/len(buylist), 0.1)
    for sec in buylist:
        order_pct_to(sec, weight)
```

BOLL 指标策略的回测结果如图 6-22 所示。可以看出，BOLL 指标策略与指数走势十分相似，基本抓住了市场上的牛市机会，且大部分时间优于指数，但同时无法有效规避风险。投资者可考虑在长线投资中使用此指标，并结合其他指标来进行风险管控。

## 图 6-22

![](img/0b89c6ed12810914fc2fb80a08af1fae_305_0.png)

## 6.7.3 CCI 指标

### 1. 指标介绍

CCI（顺势指标）由唐纳德·蓝伯特先生提出，通过引用价格与固定区间的股价平均区间的偏离程度的概念，来判断是否处于超买或者超卖状态。CCI 指标区别于大多超买超卖型指标的 0～100 界限，波动于正无穷大和负无穷大之间，无须以 0 为中轴线，有利于投资者对行情进行研判，对于短期内的暴涨暴跌的非常态行情也同样适用。

### 2. 计算公式

在计算 CCI 指标时通常需要设置计算周期 n 和系数 c，一般取 n=20，c=0.015，数值的具体计算方式为：

```
CCI_n = (Typical Price - SMA_n(TP)) / (c · δ_n(TP))
```

其中 Typical Price = (最高价+最低价+收盘价)/3

### 3. 指标使用说明

在对 CCI 指标进行分析时，我们主要对非常态市场进行研判。

（1）当 CCI 曲线向上突破+100 线进入非常态区间时，则表明股价进入强势区间，投资者应及时买入。在进入非常态区间后，如果继续一直朝上运行，则表明股价持续强势。

如果在远离 100 线的地方开始掉头，则表明强势状态已难以持续；同时，如果前期涨幅过高，则投资者应考虑逢高卖出。如果出现持续下跌状态，则表明强势趋势已经结束。

（2）当 CCI 曲线向下突破-100 线进入非常态区间时，则表明股价进入弱势区间，投资者应考虑空仓并等待更高利润。如果在超卖区间运行一段时间后开始掉头，则表明底部已初步探明，投资者可适量建仓。当由下向上突破-100 线进入常态区间时，则表明市场探底基本结束，可能进入盘整阶段，投资者可考虑逢低建仓。

通过上面的分析我们可以看出，CCI 指标主要适用于股票市场的超买与超卖区间，在相对准确性方面，对于急涨急跌的行情更适合采用 CCI 指标。

## 6.8.3.4 指标回测

下面构建基于 CCI 指标的简单策略，具体的策略要求如下。

- （1）如果 CCI 指标低于-100，则买入；如果 CCI 指标高于+100，则卖出。
- （2）对每只股票买入 20000 元左右，出现重复信号时不重复买入。
- （3）如果股票数量高于可用资金，则随机买入其中一部分，保证资金用完。
- （4）回测时间为 2016 年 1 月 1 日至 2017 年 12 月 31 日，基准为 HS300，股票池为动态 HS300 成分股，每日调仓。

上面已经介绍了如何基于通联数据平台编写技术指标策略，这里将介绍另一种基于 Talib 的技术指标策略编写，并将之与通联数据平台进行比较。

Talib 的方法代码如下：

```
import talib
import numpy as np

start = '2016-01-01'
.end = '2017-12-31'
benchmark = 'HS300'
Universe = DynamicUniverse('HS300')
capital_base = 1000000
freq = 'd'
refresh_rate = 1

N = 20  # 移动窗口长度
```

## Python 与量化投资：从基础到实战

```
buy_threshold = -100  # 买入阈值
sell_threshold = 100  # 卖出阈值

accounts = {
    'fantasy_account': AccountConfig(account_type='security', capital_base=10000000)
}

def initialize(context):
    return

def handle_data(context):
    current_universe = context.get_universe(exclude_halt=True)

    # 拿过去 N 个交易日的最高价、最低价、收盘价来估算 CCI
    data = context.history(current_universe, ['highPrice', 'lowPrice', 'closePrice'], N+1, rtype='array')

    buylist = []
    account = context.get_account('fantasy_account')
    cash = account.cash
    current_positions = account.get_positions()
    for stock in current_universe:
        cci = talib.CCI(data[stock]['highPrice'], data[stock]['lowPrice'], data[stock]['closePrice'], timeperiod=N)[-1]
        # CCI 处于超卖期，且无持仓
        if cci < buy_threshold and stock not in current_positions:
            buylist.append(stock)

        # CCI 处于超买期，且有持仓
        elif cci > sell_threshold and stock in current_positions:
            order_to(stock, 0)  # 全部卖出
            cash += current_positions[stock].amount * context.current_price(stock)  # 估计买入金额
    d = min(len(buylist), int(cash) // 20000)
    # 可以买入的股票数量，如果资金不够，则只买入其中一部分
    for stock in buylist[:d]:
        order(stock, 20000 / context.current_price(stock))
```

Uqer 方法的代码如下：

```
import numpy as np
```

```
import pandas as pd

start = '2016-01-01'
end = '2017-12-31'
benchmark = 'HS300'
 universe = DynamicUniverse('HS300')
capital_base = 1000000
freq = 'd'
refresh_rate = 1

buy_threshold = -100  # 买入阈值
sell_threshold = 100  # 卖出阈值

accounts = {
    'fantasy_account': AccountConfig(account_type='security', capital_base=10000000)
}

# 定义并注册 Signal 模块
def initialize(context):
    cci = Signal('CCI20')
    # 优矿提供了 CCI5、CCI10、CCI20、CCI88 等 4 种不同参数的预计算 CCI
    context.signal_generator = SignalGenerator(cci)

def handle_data(context):
    current_universe = context.get_universe(exclude_halt=True)
    cci = context.signal_result['CCI20']

    buylist = []
    account = context.get_account('fantasy_account')
    cash = account.cash
    current_positions = account.get_positions()
    for stock in current_universe:
        # CCI 处于超卖期，且无持仓
        if cci[stock] < buy_threshold and stock not in current_positions:
            buylist.append(stock)

        # CCI 处于超买期，且有持仓
        elif cci[stock] > sell_threshold and stock in current_positions:
            order_to(stock, 0)  # 全部卖出
```

## Python 与量化投资：从基础到实战

```
cash += current_positions[stock].amount * context.current_price(stock)  # 估计买入金额

d = min(len(buylist), int(cash) // 20000)
# 可以买入的股票数量，如果资金不够，则只买入其中一部分

for stock in buylist[:d]:
    order(stock, 20000 / context.current_price(stock))
```

两种方法的运行结果如图 6-23 所示。

![](img/0b89c6ed12810914fc2fb80a08af1fae_309_0.png)

图 6-23

从上述结果可以看出，基于两种平台的技术指标策略结果基本一致。读者可根据自身的习惯尝试直接调用通联数据技术指标库，或者基于其他数据服务商进行技术指标计算，并在通联数据平台上进行策略调试。

## 6.7.4 CMO 指标

### 1. 指标介绍

钱德动量摆动指标（CMO）由图莎尔·钱德（Tushar S.Chande）提出，利用上涨日和下跌日数据寻找极度超买和极度超卖的条件。作为动量指标，CMO 指标用来衡量证券趋势强度，以±50 为区间界限来衡量市场状态。

### 2. 计算公式

计算 CMO 指标通常需要设置计算周期 n，一般取 n=20，数值的具体计算方式为

CMO_n = (∑_{i=1}^n up_i - ∑_{i=1}^n dn_i) / (∑_{i=1}^n up_i + ∑_{i=1}^n dn_i) × 100

其中，计算周期内每一天的收盘价减去前收盘价的结果，如果结果为正，则将其赋给 up_i （dn_i 为 0）；如果结果为负，则将其绝对值赋给 dn_i （up_i 为 0）。

### 3. 指标使用说明

在对 CMO 指标进行分析时，其波动范围是-100～+100，因此我们通常有具体的定量规则对市场进行研判。

（1）CMO 指标的绝对值越高，趋势越强。趋近于 0 则表示标的证券趋向于在早水平方向波动。

（2）一般认为在 CMO 指标的值低于-50 时处于超卖状态，在高于+50 时处于超买状态，且两者的动量值为 3 倍之差。

## 6.8.4.4 指标回测

下面构建基于 CMO 指标的简单策略，具体策略要求如下。

（1）如果 CMO 指标低于-50，则买入；如果 CMO 指标高于+50，则卖出。

（2）等权买入符合条件的股票，如果可选的股票少于 10 只，则每只买入 0.1，在出现重复的信号时不重复买入。

（3）如果股票的数量超过可用资金，则随机买入其中一部分，保证资金用完。

（4）回测时间为 2012 年 1 月 1 日至 2017 年 12 月 31 日，基准为 HS300，股票池为动态 HS300 成分股，每日调仓。

Uqer 方法的代码如下：

```
import numpy as np
import pandas as pd

start = '2012-01-01'                # 回测起始时间
end = '2017-12-31'                  # 回测结束时间
universe = DynamicUniverse('HS300')
benchmark = 'HS300'
capital_base = 1000000
freq = 'd'
refresh_rate = 1

buy_threshold = -50
sell_threshold = 50

accounts = {
    'fantasy_account': AccountConfig(account_type='security', capital_base=10000000)
}

# 定义并注册 Signal 模块
def initialize(context):
    context.signal_generator = SignalGenerator(Signal('CMO'))

def handle_data(context):
    account = context.get_account('fantasy_account')
    current_universe = context.get_universe(exclude_halt=True)
    current_positions = account.get_positions()
    buylist = []
    cash = account.cash
    for sec in current_universe:
        if context.signal_result['CMO'][sec] < buy_threshold and sec not in current_positions:  # CMO 处于超卖期，且无持仓
            buylist.append(sec)
        elif context.signal_result['CMO'][sec] > sell_threshold and sec in current_positions:  # CMO 处于超买期，且有持仓
```

```
order_to(sec, 0)  # 全部卖出
cash+=current_positions[sec].amount * context.current_price(sec)
```

# 估计买入金额

```
if len(buylist)==0:
    weight = 0
else:
    weight = min(1.0/len(buylist), 0.1)
for sec in buylist:
    order_pct_to(sec, weight)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_312_0.png)

从运行结果可以看出，CMO 指标的整体表现略次于 HS300 指数，但在控制回撤方面不如指数，投资者可考虑修改离场条件，以期稳中求胜。

## 6.7.5 Chaikin Oscillator 指标

### 1. 指标介绍

佳庆指标（Chaikin Oscillator）由 Marc Chaikin 提出，是一种反映市场内在动能的动量指标。在前面提到的动量指标都是基于价格信息进行计算的，但成交量同样是一个不能忽略的重要信息来源，可以反映股价在本质上的强弱度。由于根据当日涨跌与否来将当日成交量全部看作多头或者空头的力量过于简单，所以 Chaikin Oscillator 指标针对这种方法进行了改进，计算出乘数因子和能量区间，进一步计算出 ADL 动量指标，再计算 Chaikin Oscillator 指标，并根据其符号的变化对涨跌趋势进行判断。
