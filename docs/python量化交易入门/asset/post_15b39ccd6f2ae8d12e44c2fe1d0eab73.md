
（2）过滤：ATR 指标。

（3）出场：固定百分百点位移动止损。

## 5.3.4 策略代码解析

+   1. 策略参数

ATR 指标初始化需要用到 11 天数据，即 11 天以后才能产生 ATR 值；而 ATR 移动平均（ATRMa）则是统计 10 天内的平均值。

RSI 指标规定初始化需要用到 5 天数据，16 是开仓的阈值。当 RSI > 50+16，即 RSI > 66 时，开仓做多；当 RSI < 34 时，开仓做空。这里并不用市场公认的 RSI 高于 70 做多，RSI 低于 30 做空，因为用的人多了，交易信号已经失效。

fixedSize = 1 意味着每次开平仓操作只交易 1 手。

```
# 策略参数
atrLength = 11        # 计算ATR指标的窗口数
atrMaLength = 10      # 计算ATR均线的窗口数
rsiLength = 5         # 计算RSI的窗口数
rsiEntry = 16         # RSI的开仓信号
trailingPercent = 0.8 # 百分比移动止损
initDays = 10         # 初始化数据所用的天数
fixedSize = 1         # 每次交易的数量
......
#--------------------------------------------------------------------------------
def onInit(self):
    """初始化策略（必须由用户继承实现）"""
    self.writeCtaLog(u'%s 策略初始化' %self.name)
    # 初始化RSI入场阈值
    self.rsiBuy = 50 + self.rsiEntry
    self.rsiSell = 50 - self.rsiEntry
......
```

+   2. onBar 函数

在开仓交易前，为了防止之前下的挂单在上 1 分钟没有成交，但是下 1 分钟可能已经调整了价格，就用 cancelAll()方法立刻撤销之前未成交的所有委托，保证策略在当前这 1 分钟开始的时候整个状态是清晰和唯一的。

对于本地化 K 线管理模块( am = self.am, 本地化的作用是节省不必要的缓存，提高计算速度)，若检测到 K 线管理模块还没有插入足够多的数据，即初始化状态为 False，则直接返回不进行下面的操作；反之则计算 ATR 指标、ATR 移动平均和 RSI 指标。

```
def onBar(self, bar):
    """收到 Bar 推送（必须由用户继承实现）"""
    self.cancelAll()

    # 保存K线数据
    am = self.am
    am.updateBar(bar)
    if not am.inited:
        return

    # 计算指标数值
    atrArray = am.atr(self.atrLength, array=True)
    self.atrValue = atrArray[-1]
    self.atrMa = atrArray[-self.atrMaLength:].mean()

    self.rsiValue = am.rsi(self.rsiLength)
```

+   ○ 当前无仓位，先初始化 K 线日高点和日低点的值，用的是当前 K 线的最高价和最低价。若 ATR 指标往上走超过了移动平均，说明趋势正在增强，市场波动越来越大，则用 RSI 指标来开仓操作了。RSI 的判断标准是当其指标高于 66 时，发出买入委托，用高于市价 5 个点来保证能够成交，委托数量为 1（fixedSize）。若 RSI 低于其卖出阈值，则用低于市价 5 个点来保证成交。

+   ○ 当前持有多仓，用 max()函数来统计当日 K 线达到的最高点，日低点则直接更新当前 K 线最低价。移动止损设置为：当 K 线达到日高点后，回落 0.8%时用停止单（Stop Order）进行平仓离场。

+   ○ 当前持有空仓，用 min()函数来统计当日 K 线达到的最低点，日高点则是直接更新当前 K 线最高价。移动止损设置为当 K 线达到日低点后，反弹 0.8%时用停止单（Stop Order）进行平仓离场。

```
# 判断是否要进行交易

# 当前无仓位
if self.pos == 0:
    self.intraTradeHigh = bar.high
    self.intraTradeLow = bar.low

    # ATR 数值上穿其移动平均线，说明行情短期内波动加大
    # 即处于趋势的概率较大，适合 CTA 开仓
    if self.atrValue > self.atrMa:
        # 使用 RSI 指标判断趋势行情时，会在超买超卖区钝化特征，作为开仓信号
        if self.rsiValue > self.rsiBuy:
            # 这里为了保证成交，选择超价 5 个点下单
            self.buy(bar.close+5, self.fixedSize)

        elif self.rsiValue < self.rsiSell:
            self.short(bar.close-5, self.fixedSize)

# 持有多头仓位
elif self.pos > 0:
    # 计算多头持有期内的最高价，以及重置最低价
    self.intraTradeHigh = max(self.intraTradeHigh, bar.high)
    self.intraTradeLow = bar.low

    # 计算多头移动止损
    longStop = self.intraTradeHigh * (1-self.trailingPercent/100)

    # 发出本地止损委托
    self.sell(longStop, abs(self.pos), stop=True)

# 持有空头仓位
elif self.pos < 0:
    self.intraTradeLow = min(self.intraTradeLow, bar.low)
    self.intraTradeHigh = bar.high

    shortStop = self.intraTradeLow * (1+self.trailingPercent/100)
    self.cover(shortStop, abs(self.pos), stop=True)

# 同步数据到数据库
self.saveSyncData()
```

```
# 发出状态更新事件
self.putEvent()
```

## 5.3.5 策略回测

回测相关数据设置，测试时间是 2011—2018 年，测试的品种是沪深 300 股指期货指数数据（ IF0000 ）’，故引擎参数与 Dual Thrust 策略的设置一样。

```
# 设置回测使用的数据
engine.setBacktestingMode(engine.BAR_MODE)      # 设置引擎的回测模式为 K 线
engine.setDatabase(MINUTE_DB_NAME, 'IF0000')        # 设置 IF 股指期货
engine.setStartDate('20110416')                     # 设置回测使用的数据起始日期
engine.setEndDate('20180101')                       # 设置回测使用的数据结束日期

# 配置回测引擎参数
engine.setSlippage(0.2)        # 设置滑点为股指 1 跳
engine.setRate(0.5 / 10000)    # 设置手续费为万分之 0.5
engine.setSize(300)            # 设置股指合约大小
engine.setPriceTick(0.2)       # 设置股指最小价格变动单位
engine.setCapital(1000000)     # 设置回测本金
```

初始本金为 100 万元人民币，期末本金为 199 万元人民币，年化收益为 12.8%，百分比最大回撤达−17.33%，夏普比率达 0.6，但是在 2016 年后表现不理想，存在持续亏损，如图 5-21 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_236_0.png)

## 5.3.6 滚动回测

### 1. 参数优化设置

本次参数优化的目标是夏普比率，需要优化的参数有 3 个维度，分别计算 ATR 指标所需要的回望窗口数（artLength）、ATR 平均的回望窗口数（atrMaLength）和 RSI 指标的回望窗口数（rsiLength）。其构建的优化组合为 5 × 4 × 3 = 60 个，故所需要的时间较长，若再增加一个维度的参数，优化时间就会呈指数级增长。

```
# 优化配置
setting = OptimizationSetting()              # 新建一个优化任务设置对象
setting.setOptimizeTarget('sharpeRatio')      # 设置优化排序的目标是夏普比率
setting.addParameter('atrLength', 18, 30, 2)) # 增加第一个优化参数 atrLength, 起始 # 12, 结束20, 步进2
setting.addParameter('atrMaLength', 6, 14, 2) # 增加第二个优化参数 atrMa, 起始20, 结 # 束30, 步进5
setting.addParameter('rsiLength', 4, 8, 1)     # 增加第三个优化参数 rsiLength, 起始4, # 结束8, 步进1
# 执行多进程优化
import time
start = time.time()
resultList = engine.runParallelOptimization(AtrRsiStrategy, setting)
print u'耗时: %s' %(time.time()-start)
```

### 2. 滚动回测设置

以 3 年为滚动周期，步进是半年，对 20100416 至 20180101 区间进行滚动回测及参数优化，并且收集表现最好的前 3 组优化参数。

通过对历史优化参数的统计分析，预测未来的优化参数。其选择标准如下：

+   - 近年参考权重大于远年。
- 若优化参数整体变化不大，则取众数。
- 若优化参数整体发生变化，则在近年区间取众数。
- 若前一年参数发生剧烈变化，则对该区间进行更为细致的滚动回测和参数优化。

如图 5-22 所示，历史优化参数整体发生变化，故在近年区间取众数，即 atrLength = 22， atrMaLength = 12， rsiLength = 7。

| 回测区间 | 第1优参数 | | | 第2优参数 | | | 第3优参数 | | |
|---|---|---|---|---|---|---|---|---|---|
|  | atrLength | atrMaLength | rsiLength | atrLength | atrMaLength | rsiLength | atrLength | atrMaLength | rsiLength |
| 20100416--20130501 | 30 | 6 | 7 | 22 | 6 | 7 | 20 | 6 | 7 |
| 20110101--20140101 | 20 | 8 | 7 | 30 | 6 | 7 | 22 | 6 | 7 |
| 20110601--20140601 | 20 | 8 | 7 | 30 | 6 | 7 | 24 | 8 | 7 |
| 20120101--20150101 | 20 | 10 | 7 | 22 | 8 | 7 | 20 | 10 | 7 |
| 20120601--20150601 | 22 | 8 | 7 | 24 | 12 | 7 | 22 | 8 | 7 |
| 20130101--20160101 | 30 | 6 | 7 | 28 | 6 | 7 | 28 | 10 | 7 |
| 20130601--20160601 | 30 | 6 | 7 | 20 | 6 | 7 | 24 | 12 | 7 |
| 20140101--20170101 | 22 | 12 | 5 | 24 | 12 | 5 | 30 | 10 | 6 |
| 20140601--20170501 | 22 | 12 | 7 | 34 | 12 | 7 | 30 | 10 | 7 |
| 20150101--20180101 | 22 | 12 | 5 | 24 | 12 | 5 | 28 | 12 | 5 |
| 2018至今 | 22 | 12 | 7 |  |  |  |  |  |  |

图 5-22 历史优化参数

预测优化参数历史表现是年化收益为 11.72%，百分比最大回撤为-18.89%，夏普比率达 0.58，如图 5-23 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_238_0.png)

图 5-23 预测优化参数历史表现

对 2018 年以后的预测效果为年化收益提升到 37.42%，百分比最大回撤为 -6.61%，夏普比率达 1.62，如图 5-24 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_239_0.png)

计算绩效统计结果 首个交易日：2018-01-11 最后交易日：2018-11-05 总交易日：197盈利交易日：105亏损交易日：92 初始资金：1000000 结清资金：1,307,227.78 总净利润：30.72%年化收益：37.43% 总盈亏：307,227.78最大回撤：-74,996.61 回撤百分比最大回撤：-6.61%总手续费：39,062.22总扳仓：65,610.0 总成交金额：781,244,460.0 总成交笔数：729.0平均持仓：1,569.53日均持仓数：198.29日均盈亏：333.05日均成交金额：3,965,707.92日均成交笔数：3.7日均盈亏率：0.14%收盘涨跌幅：1.31% Sharpe Ratio: 1.62

图 5-24 对 2018 年以后的预测效果

## 5.4 金肯特纳通道策略

## 5.4.1 策略原理

金肯特纳通道策略是一个典型的通道突破策略，即当价格突破通道上轨时做多，当价格走低突破通道下轨时做空，如图 5-25 所示。轨道计算的思路也相对简单，先计算移动均线（MA），并且统计 ATR 指标，设置一定的通道宽度偏差 X，如下：

+   • 上轨 = MA + X × ATR
• 下轨 = MA - X × ATR

因为 ATR 指标相对于标准差能够捕捉到 K 线跳空高开或者跳空低开的情况，所以更适合于一些在短期内有较大波动的品种，如股指期货或者有“小股指”之称的螺纹钢。

![](img/111072991fcea5fcc2790f3fbbeb60af_240_0.png)

图 5-25 金肯特纳通道图示

金肯特纳通道策略三大要素如下所示。

+   - （1）信号：价格突破上轨做多，突破下轨做空。
- （2）过滤：若价格在通道内上下走动，则不进行开仓操作。
- （3）出场：固定百分点数移动止损。

## 5.4.2 策略代码解析

### 1. 策略参数

金肯特纳通道指标初始化天数是 11 天，调用 K 线管理模块中 TA-Lib 库定义的函数可以计算出均线和 ATR 指标，进而得出金肯特纳通道的上轨和下轨，代码如下：

```
def keltner(self, n, dev, array=False):
    """金肯特纳通道"""
    mid = self.sma(n, array)
    atr = self.atr(n, array)
    up = mid + atr * dev
```

```
down = mid - atr * dev

return up, down
```

在 Keltner 函数中，入参是初始化天数（kkLeng）与通道宽度偏差（kkDev），然后把 array 设置为 True，这样就可以计算出一系列通道上轨（kkUp）和通道下轨（kkDown）。

移动止损设置为 0.8%，即在最高点回落 0.8%或者在最低点反弹 0.8%，瞬间用停止单平仓离场。策略初始化所需要载入的天数是 10，且每次交易的合约数量为 1 手。

```
# 策略参数
kkLength = 11            # 计算通道中值的窗口数
kkDev = 1.6              # 计算通道宽度的偏差
trailingPrcnt = 0.8      # 移动止损
initDays = 10            # 初始化数据所用的天数
fixedSize = 1            # 每次交易的数量
```

### 2. 新的委托方式：OCO

OCO 委托的全称是 “One-Cancels-the-Other Order”，意思是二选一委托，即在 K 线内同时发出止损买单和止损卖单：

+   • 若价格突破上轨，则触发止损买单同时取消止损卖单。
• 若价格突破下轨，则触发止损卖单同时取消止损买单。

这种挂单方式在国内交易所比较少见，一般多用于外汇市场。因为货币在短时间内会有很强的震荡，比较难以判断趋势，这时候 OCO 的优点就彰显出来了。当盘整震荡的行情接近结束，而要进入一个上涨或下跌的趋势时，可用 OCO 挂单捕捉趋势。一个例子是发生重大行情如非农或者利率决议公布，若不确定接下来的行情，则可用 OCO 挂单。

OCO 委托流程如下所述。

创建 3 个空的列表：buyOrderIDList、shortOrderIDList 和 ordList。

+   o buyOrderIDList 用于缓存止损买入单的委托，分别插入委托价格、合约手数。
o shortOrderIDList 用于缓存止损卖出单的委托，分别插入委托价格、合约手数。
o ordList 用于缓存所有发出的委托单子，用 extend()的方法把上面两个列表添加进来。

```
#-------------------------------------------------------------------
# 策略变量
buyOrderIDList = []  # OCO 委托买入开仓的委托号
shortOrderIDList = []  # OCO 委托卖出开仓的委托号
orderList = []  # 保存委托代码的列表
#-------------------------------------------------------------------
def sendOcoOrder(self, buyPrice, shortPrice, volume):
    """
    发送 OCO 委托

    OCO(One Cancel Other)委托:
    1. 主要用于实现区间突破入场
    2. 包含两个方向相反的停止单
    3. 一个方向的停止单成交后会立即撤销另一个方向的
    """
    # 发送双边的停止单委托，并记录委托号
    self.buyOrderIDList = self.buy(buyPrice, volume, True)
    self.shortOrderIDList = self.short(shortPrice, volume, True)

    # 将委托号记录到列表中
    self.orderList.extend(self.buyOrderIDList)
    self.orderList.extend(self.shortOrderIDList)
```

### 3. OCO 发单与平仓

为保证委托的唯一性，同样要撤销之前尚未成交的委托，但这一次不采用 cancelAll()方法，先在 orderList 列表中遍历出 orderID，然后删除，以保证清空 orderList 的目标。

调用 K 线管理模块 ArrayManager 里的 updateBar(bar)函数把 1 分钟 K 线数据合成 5 分钟 K 线。K 线数据若不够多，则直接返回，不进行下面的操作。

先计算金肯特纳通道的上下轨。若当前无持仓，则用最新的 5 分钟 K 线数据
