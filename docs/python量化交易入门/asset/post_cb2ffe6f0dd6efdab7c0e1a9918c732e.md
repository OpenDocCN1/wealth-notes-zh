
# 3. TurtlePortfolio 类

根据账户资金和品种合约规模，产生具体交易委托，其函数如下所示。

-   __init__(): 初始化海龟投资组合的组合市值（即账户资金）和多空头持仓，创建多个字典分别缓存海龟信号、每个品种持仓情况、交易中的信号、合约大小、单位头寸规模、真实持仓量。
- init(): 传入组合市值和合约大小字典，调用 TurtleSignal 类产生短周期版本和长周期版的交易信号（包括入场、止盈、止损），同时缓存到信号字典中。
- onBar(): 根据信号字典产生具体交易委托。
- newSignal(): 先计算单位头寸规模，若委托指令是开仓，则需要检查上一次是否赢利，无赢利就发出买入/卖空委托；若委托指令是平仓，则需要注意平仓量不能超过空头持仓，同时注意单品种和组合持仓都不能超过上限。
- sendOrder(): 计算单品种持仓和整体持仓，向回测引擎中发单记录。

# 6.3.4 海龟策略 6 大要素代码解析

海龟策略 7 大要素分别是品种选择、头寸规模、单位头寸限制、入场信号、逐步加仓、止损、止盈。由于品种选择不属于交易策略内容，故只对后面 6 大要素进行代码解析。

# 1. 头寸规模

头寸规模=(1% × 账户资金)/(ATR × 合约规模)，其代码如下：

```
class TurtlePortfolio(object):
.....
    #------------------------------------------------------------------------
    def newSignal(self, signal, direction, offset, price, volume):
.....
        riskValue = self.portfolioValue * 0.01
        multiplier = riskValue / (signal.atrVolatility * size)
        multiplier = int(round(multiplier, 0))  # multiplier 即为头寸规模
        self.multiplierDict[signal.vtSymbol] = multiplier
.....
```

```
#--------------------------------------------------------------------------------
def sendOrder(self, vtSymbol, direction, offset, price, volume, multiplier):
    """"""
    # 计算合约持仓
    if direction == DIRECTION_LONG:
        self.unitDict[vtSymbol] += volume        # volume 即为单位头寸
        self.posDict[vtSymbol] += volume * multiplier
    else:
        self.unitDict[vtSymbol] -= volume
        self.posDict[vtSymbol] -= volume * multiplier  # 实际持仓=单位头寸 × 头寸规模
```

# 2. 单位头寸限制

基于高度关联市场和松散关联市场判断起来都非常主观，并无统一标准。客观上只能实现单个市场和单个方向的头寸限制。

在开仓交易前，需要检查上一笔交易是否盈利，若盈利则直接返回，不进行买卖操作（仅仅适用于短周期版本的入场策略，即 profitCheck=True；长周期版本对应的是 profitCheck=False）。

```
MAX_PRODUCT_POS = 4        # 单品种最大持仓
MAX_DIRECTION_POS = 10     # 单方向最大持仓
.....
class TurtlePortfolio(object):
.....
    #--------------------------------------------------------------------------------
    def newSignal(self, signal, direction, offset, price, volume):
.....
        # 开仓
        if offset == OFFSET_OPEN:
            # 检查上一次是否为赢利
            if signal.profitCheck:
                pnl = signal.getLastPnl()
                if pnl > 0:
                    return
            # 买入
            if direction == DIRECTION_LONG:
                # 组合持仓不能超过上限
                if self.totalLong >= MAX_DIRECTION_POS:
                    return

                # 单品种持仓不能超过上限
                if self.unitDict[signal.vtSymbol] >= MAX_PRODUCT_POS:
                    return

            # 卖出
            else:
                if self.totalShort <= -MAX_DIRECTION_POS:
                    return
                    
                if self.unitDict[signal.vtSymbol] <= -MAX_PRODUCT_POS:
                    return
```

# 3. 入场信号、逐步加仓、止损、止盈

原版海龟策略提供两个版本的入场和止盈信号，分别是长周期版本和短周期版本的唐奇安通道突破规则。

-   短周期版本：入场用的是 20 日唐奇安通道，止盈用的是 10 日唐奇安通道，用 20 日周期计算 ATR 值，有上一笔赢利当前信号无效的过滤条件。
-   长周期版本：入场用的是 55 日唐奇安通道，止盈用的是 20 日唐奇安通道，用 20 日周期计算 ATR 值，无上一笔赢利当前信号无效的过滤条件。

逐步加仓的规则是每隔 0.5×ATR 幅度慢慢加满至 4 个单位头寸，止损也相应根据 0.5×ATR 的步进移动。

```
class TurtleSignal(object):
    #------------------------------------------------------------------------------
    def __init__(self, portfolio, vtSymbol,
                   entryWindow, exitWindow, atrWindow,
                   profitCheck=False):
    .....
    #------------------------------------------------------------------------------
    def onBar(self, bar):
        self.bar = bar
        self.am.bar
        if not self.am.inited:
            return

        self.generateSignal(bar)
```

```
self.calculateIndicator()
#----------------------------------------------------------------------
def generateSignal(self, bar):
    """
判断交易信号
"""
# 如果指标尚未初始化, 则忽略
if not self.longEntry1:
    return

# 优先检查平仓
if self.unit > 0:
    longExit = max(self.longStop, self.exitDown)

    if bar.low <= longExit:
        self.sell(longExit)
        return
elif self.unit < 0:
    shortExit = min(self.shortStop, self.exitUp)
    if bar.high >= shortExit:
        self.cover(shortExit)
        return

# 没有仓位或者持有多头仓位时, 可以做多 (加仓)
if self.unit >= 0:
    trade = False

    if bar.high >= self.longEntry1 and self.unit < 1:
        self.buy(self.longEntry1, 1)
        trade = True

    if bar.high >= self.longEntry2 and self.unit < 2:
        self.buy(self.longEntry2, 1)
        trade = True

    if bar.high >= self.longEntry3 and self.unit < 3:
        self.buy(self.longEntry3, 1)
        trade = True

    if bar.high >= self.longEntry4 and self.unit < 4:
        self.buy(self.longEntry4, 1)
        trade = True

if trade:
    return

# 没有仓位或者持有空头仓位时，可以做空（加仓）
if self.unit <= 0:
    if bar.low <= self.shortEntry1 and self.unit > -1:
        self.short(self.shortEntry1, 1)

    if bar.low <= self.shortEntry2 and self.unit > -2:
        self.short(self.shortEntry2, 1)

    if bar.low <= self.shortEntry3 and self.unit > -3:
        self.short(self.shortEntry3, 1)

    if bar.low <= self.shortEntry4 and self.unit > -4:
        self.short(self.shortEntry4, 1)

#--------------------------------------------------------
def calculateIndicator(self):
    """计算技术指标"""
    self.entryUp, self.entryDown = self.am.donchian(self.entryWindow)
    self.exitUp, self.exitDown = self.am.donchian(self.exitWindow)

# 有持仓后, ATR波动率和入场位等不再变化
if not self.unit:
    self.atrVolatility = self.am.atr(self.atrWindow)

    self.longEntry1 = self.entryUp
    self.longEntry2 = self.entryUp + self.atrVolatility * 0.5
    self.longEntry3 = self.entryUp + self.atrVolatility * 1
    self.longEntry4 = self.entryUp + self.atrVolatility * 1.5
    self.longStop = 0

    self.shortEntry1 = self.entryDown
    self.shortEntry2 = self.entryDown - self.atrVolatility * 0.5
    self.shortEntry3 = self.entryDown - self.atrVolatility * 1
    self.shortEntry4 = self.entryDown - self.atrVolatility * 1.5
    self.shortStop = 0

......
class TurtlePortfolio(object):
```

```
#-----------------------------------------------
def __init__(self, portfolioValue, vtSymbolList, sizeDict):
    """"""
    self.portfolioValue = portfolioValue
    self.sizeDict = sizeDict

    for vtSymbol in vtSymbolList:
        signal1 = TurtleSignal(self, vtSymbol, 20, 10, 20, True)
        signal2 = TurtleSignal(self, vtSymbol, 55, 20, 20, False)

        l = self.signalDict[vtSymbol]
        l.append(signal1)
        l.append(signal2)
        self.unitDict[vtSymbol] = 0
        self.posDict[vtSymbol] = 0

#--------------------------------------------------
def onBar(self, bar):
    """"""
    for signal in self.signalDict[bar.vtSymbol]:
        signal.onBar(bar)
```

# 6.3.5 海龟策略的回测

-   1. 运行 run.ipynb 文件

首先进入 “examples\TurtleStrategy” 文件夹，在 Jupyter Notebook 中打开 run.ipynb 即可执行策略回测。

（1）调用海龟回测引擎。

```
%matplotlib inline
from datetime import datetime
import Numpy as np
import matplotlib.pyplot as plt
from turtleEngine import BacktestingEngine
```

（2）设置回测时间区间和起始资金，读取 csv 文件的合约信息进行策略回测，

# Python 量化交易

然后显示逐日统计的相关指标和资金图。

```
engine = BacktestingEngine()
engine.setPeriod(datetime(2014, 1, 1), datetime(2018, 12, 30))
engine.initPortfolio('setting.csv', 10000000)

engine.loadData()
engine.runBacktesting()
engine.showResult()
```

（3）针对投资组合里的单个品种，查阅其逐步开平仓记录。

```
tradeList = engine.getTradeData('J99')
for trade in tradeList:
    print '%s %s %s %s@%s' %(trade.vtSymbol, trade.direction, trade.offset,
                           trade.volume, trade.price)
```

# 2. 配置 JSON 文件

在运行海龟策略回测时会读取同一文件夹内的 csv 文件，下面以 setting.csv 为例说明一下，如图 6-8 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_300_0.png)

需要配置的合约信息包括合约品种、合约规模、最小价格变动、手续费率（如每一手 0.00003）、固定手续费（如每一手 12 元）、滑点。其中手续费率与固定手续费可二选一。以 PTA 合约为例，其品种信息为 TA99¹，合约规模是 5 吨，最小价格变动是 2 元/吨，手续费率为 0，固定手续费为 12 元，滑点为 2 元。

> 1 《海龟交易法则》明确表示其交易信号源于期货指数合约，故用 “99” 结尾的 RQData 合约进行策略回测。

# 6.4 品种选择验证

# 6.4.1 原版投资组合测试

原版海龟策略选择标准主要是流动性强，若简单地理解为交易所成交量巨大的热门品种，则根据交易所分类所构建的组合历史回测如图 6-9 所示（测试环境是无手续费、无滑点）。图 6-9 中显示，上期所热门品种组合夏普比率达到 1.01，郑商所的达到 0.8，大商所的达到 1.31，而中金所因为只有 IF 股指期货成交量较高，故中金所只测试了一个品种，其夏普比率达到 0.88。总体来看，原版海龟测试夏普比率都不错，有一定的稳健性。

![](img/111072991fcea5fcc2790f3fbbeb60af_301_0.png)

图 6-9 四大交易所热门品种组合历史回测

因为国内四大交易所包含了金融产品、工业品、农业品、金属、化工等不同品种分类，为了分散投资组合各个头寸的风险，提高组合的夏普比率，故海龟策略投资组合品种必须涵盖四大交易所的品种，现在简单地把四大交易所热门品种组合起来进行测试，其效果如图 6-10 所示。
