
（4）中国金融交易所（简称中金所）品种，如表 7-4 所示。

## 表 7-4 中金所品种

| 交易所 | 品种 | 代码 | 交易所手续费费率 | 日内单双边 | 平今手续费费 |
| --- | --- | --- | --- | --- | --- |
| 中国金融交易所 | 沪深300 | IF | 万0.23 | 双边 | 万6.9 |
|  | 中证500 | IC | 万0.23 | 双边 | 万6.9 |
|  | 上证50 | IH | 万0.23 | 双边 | 万6.9 |
|  | 10年国债 | T | 3元 | 单边 |  |
|  | 5年国债 | TF | 3元 | 单边 |  |
|  | 2年国债 | TS | 3元 | 单边 |  |

从表 7-1 到表 7-4 中可以发现平仓手续费比开仓手续费高，其原因是为了防止过度投机。其中，中金所甚至启动平今惩罚机制，其开仓手续费为万分之 0.23，但是平仓手续费高达万分之 6.9。为了规避这种惩罚机制，需要用锁仓来代替平今。下面来对比这两种方式的特点。

正常日内开仓-平仓手续费率如下。

- 多头开仓手续费率为 0.0023%。
- 日内平多的手续费率为 0.069%。

故当日开平仓一次，损失的手续费率为 0.0023% + 0.069% = 0.00713%。可以通过锁仓规避高昂的平今仓手续费，即把日内平多变成空头开仓，并且在第二天开盘后平掉昨天的多仓与空仓：

- 多头开仓手续费为 0.0023%。
- 空头开仓手续费为 0.0023%。
- 多头平昨手续费为 0.0023%。
- 空头平昨手续费为 0.0023%。

当日平仓改为锁仓模式，手续费降低至 0.0092%。由于 vn.py 可以用底层算法进行锁仓，大大增加了策略开发的简便性。

在回测中，锁仓模式交易笔数是平仓手数的 2 倍，但考虑到加入锁仓模式规避手续费并且交易手数不变，可以把股指期货手续费调整到 0.005%。

最后应该注意中金所在计算平今交易手续费时，对当日平仓成交按成交时间先后顺序，逐笔采取“先平当日新开仓，再平历史仓”的方式计算平今数量，每笔成交的平今数量为该笔成交中平当日新开仓数量。故在无今仓情况下，应该先平掉昨仓，再开今仓。

## 3. 底层委托转换

在 vn.py 的 vtEngine 中实现本地持仓数据缓存功能：基于持仓查询、成交推送、委托挂撤单来实时计算当前所有仓位的持仓量、冻结量和可用量的数据。通过缓存对象实现委托开平仓自动转换功能，将上层的开平仓请求根据实时持仓情况转化成底层需要发出的委托，主要提供以下三种模式。

（1）普通模式（MODE_NORMAL）：

- 什么转换都不做，直接发出。
- 大部分合约默认使用该模式。

（2）上期所模式（MODE_SHFE）：

- 将平仓委托自动根据今仓与昨仓情况拆分，优先平今仓，今仓不足则平昨仓。
- 上期所合约默认使用该模式。

（3）平今惩罚模式（MODE_TDPENALTY）：

- 反向有昨仓且无今仓时优先平昨仓。
- 反向有今仓或无昨仓时改为开仓。
- 禁止平今仓。
- 需要在 “vnpy-1.9.0\examples\VnTrader” 里的 VT_setting.json 里配置使用该模式的合约，其配置如图 7-9 所示。

```
{
  "fontFamily": "微软雅黑",
  "fontSize": 12,
  "mongoHost": "localhost",
  "mongoPort": 27017,
  "mongoLogging": false,
  "darkStyle": false,
  "language": "chinese",
  "logActive": true,
  "logLevel": "debug",
  "logConsole": false,
  "logFile": false,
  "tdPenalty": ["IFM", "THF", "TCH"],
  "maxDecimal": ...
}
```

图 7-9 设置平今惩罚模式

下面展示底层委托转换函数。

```
def convertOrderReq(self, req):
    """转换委托请求"""
    # 普通模式无须转换
    if self.mode is self.MODE_NORMAL:
        return [req]

    # 上期所模式拆分今昨, 优先平今
    elif self.mode is self.MODE_SHFE:
        # 开仓无须转换
        if req.offset is OFFSET_OPEN:
            return [req]

        # 多头
        if req.direction is DIRECTION_LONG:
            posAvailable = self.shortPos - self.shortPosFrozen
            tdAvailable = self.shortTd - self.shortTdFrozen
            ydAvailable = self.shortYd - self.shortYdFrozen
        # 空头
        else:
            posAvailable = self.longPos - self.longPosFrozen
            tdAvailable = self.longTd - self.longTdFrozen
            ydAvailable = self.longYd - self.longYdFrozen

        # 平仓量超过总可用, 拒绝, 返回空列表
```

```
if req.volume > posAvailable:
    return []
# 平仓量小于今可用, 全部平今仓
elif req.volume <= tdAvailable:
    req.offset = OFFSET_CLOSETODAY
    return [req]
# 平仓量大于今可用, 先平今仓再平昨仓
else:
    l = []
    if tdAvailable > 0:
        reqTd = copy(req)
        reqTd.offset = OFFSET_CLOSETODAY
        reqTd.volume = tdAvailable
        l.append(reqTd)
    reqYd = copy(req)
    reqYd.offset = OFFSET_CLOSEYESTERDAY
    reqYd.volume = req.volume - tdAvailable
    l.append(reqYd)
    return l

# 平今惩罚模式, 没有今仓则平昨仓, 否则锁仓
elif self.mode is self.MODE_TDPENALTY:
    # 多头
    if req.direction is DIRECTION_LONG:
        td = self.shortTd
        ydAvailable = self.shortYd - self.shortYdFrozen
    # 空头
    else:
        td = self.longTd
        ydAvailable = self.longYd - self.longYdFrozen

# 这里针对开仓和平仓委托使用同一套逻辑

# 如果有今仓, 则只能开仓 (或锁仓)
if td:
    req.offset = OFFSET_OPEN
    return [req]
# 如果平仓量小于昨可用量, 全部平昨仓
elif req.volume <= ydAvailable:
```

```
if self.exchange is EXCHANGE_SHFE:
    req.offset = OFFSET_CLOSEYESTERDAY
else:
    req.offset = OFFSET_CLOSE
return [req]
# 平仓库大于昨日可用量，平仓再反向开仓
else:
    l = []

    if ydAvailable > 0:
        reqClose = copy(req)
        if self.exchange is EXCHANGE_SHFE:
            reqClose.offset = OFFSET_CLOSEYESTERDAY
        else:
            reqClose.offset = OFFSET_CLOSE
        reqClose.volume = ydAvailable

        l.append(reqClose)

        reqOpen = copy(req)
        reqOpen.offset = OFFSET_OPEN
        reqOpen.volume = req.volume - ydAvailable
        l.append(reqOpen)

    return l

# 其他情况则直接返回空
return []
```

## 7.3 模拟测试

### 7.3.1 策略文件目录

VnTrader 的 CTA 策略模块，支持加载当前运行时目录下的策略文件。注意策略文件必须以小写的 strategy 开头（如 strategyMultiTimeframeRu.py），同时文件中的策略类名称中必须包含大写的 Strategy（如 MultiTimeframeRuStrategy）。

现在将投资组合的 3 个策略文件放在 “vnpy-1.9.0\examples\VnTrader” 文件夹内，策略类分别命名为 MultiTimeframeRuStrategy，BollChannelRbStrategy 和 DTI fStrategy，然后配置好 CTA setting.json 文件即可。

### 7.3.2 实盘/模拟盘配置文件

对于同一个策略类；可以通过设定不同的参数和交易品种来创建不同的实例，用户可以在 “vnpy-1.9.0\examples\VnTrader” 文件夹内的 CTA_setting.json 中配置实例。

每个实例的配置项如下所述。

- ○ name：实例名字，不能重复。
- ○ className：策略类名，基于具体的策略类来创建新的 “子示例”。命名规则可以是 “策略类_品种” + “Strategy”，如 BollChannel_RbStrategy。
- ○ vtSymbol：实例交易的品种。
- ○ 策略中定义的参数，在这里没有给出的参数将使用策略里赋予的默认值。

因此，配置内容如图 7-10 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_364_0.png)

打开 VnTrader，连接交易接口后，单击菜单栏的“功能→CTA 策略”可以打开 CTA 策略模块的实盘/模拟盘交易管理界面，如图 7-11 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_365_0.png)

图 7-11 实盘/模拟盘交易管理界面

其中各区域作用如下：

- ○ 单击“加载策略”按钮，策略引擎会读取 CTA_setting.json 中的策略配置，生成具体的策略实例（下面称为“策略”），并显示在管理界面上。○ 单击“初始化”按钮会调用策略的 onInit 函数，并完成策略开始交易前的初始化操作，此时策略可以计算参数指标等，但是无法发出委托。○ 单击“启动”按钮会调用策略的 onStart 函数，正式启动策略的交易功能，此时策略基于收到的行情、成交、委托等数据进行相应的逻辑判断，并发出委托。

- • 单击 “停止” 按钮会调用策略的 onStop 函数，撤销当前策略已发出的所有未成交委托，并停止策略的交易功能，恢复到无法发出委托的状态。
- • 如果加载了多个策略，窗口最上方的 “全部初始化” “全部启动” 和 “全部停止” 可以一次性对所有的策略执行相应的操作，省去多次点击。
- • 策略参数栏用于显示策略参数、状态，以及策略中指定输出的中间变量值，以监控策略运行。
- • 最下方的 Log 输出窗口，策略可调用 writeCtaLog() 函数在这里输出。

另外，对于同一个策略类，可以通过设定不同的参数和交易品种来创建不同的实例，以跨时间周期策略 MultiTimeframeStrategy 为例，下面定义了 MultiTimeframeStrategy 策略的两个实例：一个用于交易螺纹钢 Rb，另一个用于交易橡胶 Ru，它们的名称 name 与具体策略参数都不同。如图 7-12 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_366_0.png)

实现效果如图 7-13 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_367_0.png)

图 7-13 监控组件的多策略实例

## 7.4 真实交易环境

### 7.4.1 交易环境的 3 套系统

国内市场存在 3 套系统：交易所系统、经纪商柜台系统和投资者交易系统。这 3 套系统启动是有先后顺序的，一定是交易所系统先启动完毕后，再启动经纪商柜台系统，最后才启动投资者的交易系统。

- 交易所系统：先启动核心交易，比如撮合队列的报盘前置机；然后启动行情服务器，用于不断获取交易所撮合队列的实时队列并推送到交易所系统行情客户端。
- - 经纪商柜台系统：经纪商主要指期货公司和证券公司，而经纪商柜台系统指的是就是 CTP、飞马、飞创等柜台系统。其主要功能包括先启动柜台服务器，用于期货公司接受客户订单，进行验资验券，前端风控等；然后启动经纪商行情服务器，用于交易所系统行情客户端，把行情推送到经纪商行情服务器上。关于行情服务器，不同技术公司有不同优势，例如郑商所易盛公司开发的易盛柜台对郑商所品种有速度的优势；飞马柜台则对股指期货品种进行专门的优化。
- - 投资者交易系统：先初始化交易系统，连接柜台和行情服务器，确认上一个交易日的结算报告，查询合约、资金、持仓数据，订阅合约行情。然后初始化策略应用模块，例如在 CTA 策略交易模块上，分别单击加载策略和初始化策略启动策略。正常情况是，在下午收盘后 15~20 分钟最后一个结算 Tick 推送完毕，就可以停止策略。若不停止策略，在非交易时段就会从柜台系统那边收到一堆奇怪的行情数据，这是因为经纪商有可能测试系统或者升级系统，从而推送异常数据。

### 7.4.2 交易环境的数据流

交易所系统发出 Tick 行情，中间经过经纪商柜台系统转发到投资者交易系统上的这一过程根据硬件设备的不同，传递时间也不相同。若投资者的机器在托管机房，则这个时间大概有几十微秒到几百微秒；若从公网接入，那么时间可以从几毫秒增长到几十毫秒，这是最慢，也是最常见的方案，即从网卡接收数据，途径内核到用户态内存。

从用户态内存拿到数据，在投资者交易系统内做计算，然后发出订单，订单指令到网卡，通过网络到达经纪商柜台系统，经纪商柜台系统会先进行验资验券：不符合条件则直接拒单；符合条件把委托发送到交易所系统，插入中央订单簿等待撮合成交。之后，交易所系统的回报信息也会通过经纪商柜台系统推送到投资者交易系统里，如图 7-14 所示。

![](img/111072991fcea5fcc2790f3fbbeb60af_369_0.png)

# 图 7-14 交易环境的数据流

## 7.5 实际操作注意事项

在模拟盘中，用户可能发现现实交易的情况远没有回测时出色，下面归纳出7点原因。（通过 vn.py 回测已杜绝未来函数出现的可能性，因为从数据库里载入数据后，会逐条推送到策略中，以模拟真实交易环境。）

### 7.5.1 计算错误

在回测中需要设置合约规模、最小价格变动、滑点、手续费等。若 1 个甚至多个参数设置错误，则会导致回测出现巨大的误差。比如回测沪深 300 股指期货行情数据时却用 релье螺纹钢合约的数据，尽管在回测中或许能得到非常喜人的夏普比率，但是在仿真/实盘交易中肯定亏钱。

正确设置如下：

```
engine.setBacktestingMode(engine.BAR_MODE)      # 设置引擎的回测模式为K线
engine.setDatabase(MINUTE_DB_NAME, 'IF0000')  # 设置沪深300股指期货 1分钟K线数据
engine.setStartDate('20100416')                 # 设置回测用数据起始日期

engine.setSlippage(0.2)      # 设置滑点为股指1跳
engine.setRate(0.5/10000)    # 设置手续费万0.5
engine.setSize(300)          # 设置股指合约大小，IF股指是300元/点
engine.setPriceTick(0.2)     # 设置股指最小价格变动，沪深300股指最新价格变化单位是0.2
engine.setCapital(1000000)   # 设置回测本金为100万元
```

错误设置如下：

```
engine.setBacktestingMode(engine.BAR_MODE)      # 设置引擎的回测模式为K线
engine.setDatabase(MINUTE_DB_NAME, 'IF0000')  # 设置沪深300股指期货 1分钟K线数据
```

```
engine.setStartDate('20100416')                # 设置回测用的数据起始日期
engine.setSlippage(1)      # 设置滑点为股指 1 跳
engine.setRate(1/1000)     # 设置手续费万 10
engine.setSize(10)         # 设置股指合约大小
engine.setPriceTick(1)     # 设置股指最小价格变动
engine.setCapital(1000000) # 设置回测本金为 100 万元
```

### 7.5.2 数据使用误差

在期货领域，策略回测用的是期货指数生成信号，交易的是期货指数，但是在实际中用主力连续合约生成信号，交易的也是主力连续合约。两者有一定的区别，故实际效果会比策略回测时要差一些。

## 1. 主力连续合约

主力连续合约由期货品种中持仓量最大的合约的 K 线连接而成，它最能代表当前行情的走势，但随着交割期的接近，大量的投机资金会退出并转移到另一个合约，形成新的主力合约。图 7-15 展示的是天勤终端把每一时期主力合约的走势拼成一幅连续图，所以这就造成换月时的价格跳空。

![](img/111072991fcea5fcc2790f3fbbeb60af_370_0.png)

图 7-15 主力连续合约跳空
