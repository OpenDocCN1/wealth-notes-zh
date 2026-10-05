
在输出信息中，2018年12月18日设置了一个买单操作，2018年12月19日执行了买单操作，买卖点如图11-8所示。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_280_0.png)

图11-8 买卖点

## 第12章 买卖点分析

本章主要内容如下。

- 案例：买卖点设置。
- 优化输出信息。
- 案例：手动版策略参数优化。

### 12.1 案例：买卖点设置

本节案例程序文件名是： btr_e022tradExt.py，介绍买卖点的操作。
以下是案例程序，main主流程部分的代码：

```
print('\n#1,设置 BT 量化回测程序入口')
cerebro = bt.Cerebro()

#
print('\n#2,设置 BT 回测初始参数: 起始资金等')
dmoney0=100000.0
cerebro.broker.setcash(dmoney0)
dcash0=cerebro.broker.startingcash

#
#
print('\n\t#2-2,设置数据文件, 需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fdCarrier: not a code
fdat=rs0+xcod+'.csv'
print('\t@数据文件名: ',fdat)

#
print('\t设置数据 BT 回测运算: 起始时间、结束时间')
print('\t数据文件, 可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t格式为: 标准 OHLC 格式, 可以是日线、分钟数据')
t0str,t9str='2018-10-01','2018-12-31'
data=tq.pools_get4fn(fdat,t0str,t9str)

#
# 增加一个股票名称变量 xcod, 可以在策略函数、绘图中使用
cerebro.adddata(data,name=xcod)
#
#
```

```
print('\n\t#2-3,添加 BT 量化回测程序对应的策略参数')
print('\n\t# 案例中使用的是 MA 均线策略')
#cerebro.addstrategy(TQSta001,maperiod=5)
cerebro.addstrategy(TQSta001,maperiod=5,fgPrint=True)
#
print('\n\t#2-4,添加经纪人佣金，默认为：千一')
cerebro.broker.setcommission(commission=0.001)
#
print('\n\t#2-5,设置每手交易数目为: 10, 不是默认值: 1 手')
cerebro.addsizer(bt.sizers.FixedSize, stake=10)

print('\n\t#2-5,设置 addanalyzer 分析参数')

cerebro.addanalyzer(SQN)
#
cerebro.addanalyzer(bt.analyzers.SharpeRatio, _name = 'SharpeRatio', legacyannual=True)
cerebro.addanalyzer(bt.analyzers.AnnualReturn, _name='AnnualReturn')
#
cerebro.addanalyzer(bt.analyzers.TradeAnalyzer, _name='TradeAnalyzer')
cerebro.addanalyzer(bt.analyzers.DrawDown, _name='DW')

#
print('\n#3,调用 BT 回测入口程序，开始执行 run 量化回测运算')
results=cerebro.run()

#
print('\n#4,完成 BT 量化回测运算')
dval9=cerebro.broker.getvalue()
dget=dval9-dcash0
kret=dget/dcash0*100
# 最终投资组合价值
print('\t起始资金 Starting Portfolio Value: %.2f' % dcash0)
print('\t资产总值 Final Portfolio Value: %.2f' % dval9)
print('\t利润总额: %.2f,' % dget)
```

```
print('\t ROI 投资回报率 Return on investment: %.2f %%' % kret)

#
#---------
print('\n#8,Analyzer 分析BT 量化回测数据')
strat =results[0]
anzs=strat.analyzers
#
dsharp=anzs.SharpeRatio.get_analysis()['sharperatio']
trade_info=anzs.TradeAnalyzer.get_analysis()
#
dw=anzs.DW.get_analysis()
max_drowdown_len =dw['max']['len']
max_drowdown =dw['max']['drawdown']
max_drowdown_money =dw['max']['moneydown']
#
print('\t 夏普指数 SharpeRatio : ',dsharp)
print('\t 最大回撤周期 max_drowdown_len : ', max_drowdown_len)
print('\t 最大回撤 max_drowdown : ', max_drowdown)
print('\t 最大回撤(资金)max_drowdown_money : ', 
            max_drowdown_money)

#---------
#
print('\n#9,绘制BT 量化分析图形')
#cerebro.plot(style='line')
#cerebro.plot(style='candle')
cerebro.plot(style='bar')

#---------
#
print('\nzok')
```

买卖点就是买卖时机。

下面看一看买卖点的设置。在策略的next函数源码中，与买卖点设置相关的代码如下：

```
def next(self):
...
if not self.position:
    if self.dataclose[0] < self.sma[0]:
        self.log('设置买单 BUY CREATE, %.2f, name : %s'
        % (self.dataclose[0],self.datas[0]._name))
        self.order = self.buy()
else:
    if self.dataclose[0] > self.sma[0]:
        self.log('设置卖单 SELL CREATE, %.2f, name : %s'
        % (self.dataclose[0],self.datas[0]._name))
        self.order = self.sell()
```

将当天收盘价与MA均线价格进行比较，再分别进行买单操作和卖单操作。

买卖点的执行就是买单和卖单的执行，它们是在量化程序后台自动运行的。当市场出现和订单匹配的价格时，订单会自动执行并通过订单管理函数中的notify_order订单通知函数生成输出信息。

以下是notify_order订单通知函数中与订单执行相关的主要代码：

```
def notify_order(self, order):
    ....
    if order.status in [order.Completed]:
        if order.isbuy():
            self.log('买单执行 BUY EXECUTED,成交价: %.2f,小计 : %.2f,佣金%.2f' % (order.executed.price,order.executed.value,order.executed.comm))
            self.buyprice = order.executed.price
            self.buycomm = order.executed.comm
        elif order.issell():
            self.log('卖单执行 SELL EXECUTED,成交价: %.2f,小计: %.2f,佣金%.2f' % (order.executed.price,order.executed.value,order.executed.comm))
            self.bar_executed = len(self)
    ....
    self.order = None
```

如图12-1所示是BackTrader量化软件中的notify_order订单通知函数关系属性示意图。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_287_0.png)

图12-1 notify_order订单通知函数关系属性示意图

由图12-1可以看出，该函数属于Strategy策略类函数，主要相关的子模块库有Analyzer分析模块等。

### 12.2 优化输出信息

大家可能已经注意到，案例程序的输出信息非常多，而且非常混乱，比如：

```
.....
2018-11-29, 当前收盘价Close, 6.26
2018-11-29, 设置买单 BUY CREATE, 6.26, name : 002046
2018-11-30, 买单执行 BUY EXECUTED,成交价: 6.30,小计 Cost: 63.00,佣金 Comm 0.06
2018-11-30, 当前收盘价Close, 6.27
2018-12-03, 当前收盘价Close, 6.90
2018-12-03, 设置卖单 SELL CREATE, 6.90, name : 002046
2018-12-04, 卖单执行 SELL EXECUTED,成交价: 7.12,小计 Cost: 63.00,佣金 Comm 0.07
2018-12-04, 交易操盘利润OPERATION PROFIT, 毛利GROSS 8.20, 净利NET 8.07
2018-12-04, 当前收盘价Close, 7.02
2018-12-05, 当前收盘价Close, 6.84
2018-12-06, 当前收盘价Close, 6.90
```

```
2018-12-07, 当前收盘价Close, 7.59
2018-12-10, 当前收盘价Close, 7.06
.....
```

以上输出信息中，有些只有每天的收盘价，属于可以省略的无效信息。

在进行实盘交易时并不需要输出每天的价格信息，只有在实际交易日有交易订单设置、执行时，才需要输出价格信息。

可以屏蔽next函数开头部分的log输出语句，取消输出信息当中

的“每天的收盘价”，或者修改log函数中的fgPrint变量：

```
class TQSta001(bt.Strategy):
    # 定义MA均线策略的周期参数变量，默认值是15
    # 增加一个log打印开关变量fgPrint，默认是关闭状态
    params = (
        ('maperiod', 15),
        ('fgPrint', True),
    )
```

在策略定义头部代码时，要把params策略参数中的fgPrint默认值改为False。

以下是策略内定义的log输出函数代码：

```
def log(self, txt, dt=None, fgPrint=False):
    # 增强型log记录函数，带fgPrint打印开关变量
    if self.params.fgPrint or fgPrint:
        dt = dt or self.datas[0].datetime.date(0)
        print('%s, %s' % (dt.isoformat(), txt))
```

也可以在添加策略时，在主流程中把fgPrint参数设置为False：

```
cerebro.addstrategy(TQSta001,maperiod=5, fgPrint=False)
```

这样设置后再运行程序，输出信息就精简了很多：

```
.....

2018-11-29, 卖单执行 SELL EXECUTED,成交价: 6.51,小计 Cost: 63.00,佣金 Comm 0.07
```

```
2018-11-29, 设置买单 BUY CREATE, 6.26, name : 002046
2018-11-30, 买单执行 BUY EXECUTED,成交价： 6.30,小计 Cost: 63.00,佣金 Comm 0.06
2018-12-03, 设置卖单 SELL CREATE, 6.90, name : 002046
2018-12-04, 卖单执行 SELL EXECUTED,成交价： 7.12,小计 Cost: 63.00,佣金 Comm 0.07
......
```

注意输出信息当中的买单信息、卖单信息，包括订单的设置信息、执行信息。

订单的执行由系统在后台操作，当市场价格和订单设置的价格匹配时，订单才会执行，然后检查相关参数，对于符合条件的订单，系统就会确认订单的执行操作，并输出相关的订单执行信息。

需要说明的是，必须在执行前完成对订单的设置。但订单执行的时间不一定是订单设置的第二天，因为订单成交需要一定的时间。

下面以2018年12月的交易数据为例，对买卖点进行分析，相关的输出信息如下：

```
2018-12-03, 设置卖单 SELL CREATE, 6.90, name : 002046
2018-12-04, 卖单执行 SELL EXECUTED,成交价:7.12,小计 Cost:63.00,佣金 Comm:0.07
2018-12-10, 设置买单 BUY CREATE, 7.06, name : 002046
2018-12-11, 买单执行 BUY EXECUTED,成交价:7.07,小计 Cost:70.70,佣金 Comm:0.07
2018-12-11, 设置卖单 SELL CREATE, 7.15, name : 002046
2018-12-12, 卖单执行 SELL EXECUTED,成交价:7.06,小计 Cost:70.70,佣金 Comm:0.07
2018-12-12, 设置买单 BUY CREATE, 7.06, name : 002046
2018-12-13, 买单执行 BUY EXECUTED,成交价:7.13,小计 Cost:71.30,佣金 Comm:0.07
2018-12-13, 设置卖单 SELL CREATE, 7.36, name : 002046
2018-12-14, 卖单执行 SELL EXECUTED,成交价:7.20,小计 Cost:71.30,佣金 Comm:0.07
2018-12-14, 设置买单 BUY CREATE, 6.95, name : 002046
2018-12-17, 买单执行 BUY EXECUTED,成交价:6.83,小计 Cost:68.30,佣金 Comm:0.07
```

输出的买卖点结果如图12-2所示。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_291_0.png)

图12-2 买卖点结果图

图中所显示的买卖点信息是订单执行日期，而不是订单设置日期，表示产生实际交易的日 期，因为在买单和卖单设置后，不一定就能够执 行。例如，用户想在6.9元这个价格买入，但未来几天，市场价格都在7 元以上，以至于买入订单始终未能执行，这个现象称为滑点。在一些策 略中，如果订单超过n天没有被执行，则需要及时回收，这类似于止损 操作。

案例运行结果图上的买卖点信息存在一个bug：图中的买卖点图标对应的时间信息与实际订单执行的时间信息之间会出现偏差。

在很多案例中，经常会出现一些问题，比如：

- • 图中没有第一次交易的买卖点标志。
- • 买卖点的交易时间信息和订单执行时间有偏差。

之所以会出现这些问题，笔者认为可能是受到了时区影响，例如，美股交易通常采用的是纽约时间，美国的时区设置本身就比较复杂，包

括多种时区，例如太平洋时区、纽约时区等，跨度比较大；中国的时区跨度也很大，但只以北京时间为准。另外，还有一个原因，可能就是交易日历的问题，每个国家的风俗习惯不同，节假日不同，交易日历就不同。

如图12-3所示是BackTrader量化软件中的Tradingcal交易日历子模块关系属性示意图。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_292_0.png)

图12-3 Tradingcal交易日内子模块关系属性示意图

由图12-3可以看出，其内置了周一至周日的判定函数，以及ISO模式的相关函数。此外，还集成了Pandas的市场日历模块库，这也属于第三方模块库。

欧美一些国家一般在圣诞节休息，中国有春节、中秋节等节假日，而且中国的节日部分是农历假期，其对应的公历日期每年都在变化，所以需要人工每年进行修订。相当于每年都需要对交易日历进行手动修订来填写相关数据。

虽然交易日历比较烦琐，但实际上对实盘交易的影响不大，因为对于所有交易者来说都是一样的。

### 12.3 案例： 手动版策略参数优化

本节案例程序文件名是： btr_e022tradExt2.py，通过手动修改MA均线策略当中的时间周期参数maperiod来测试回报率的变化，从而挑选出回报率较高的参数，作为MA均线策略中时间参数变量的maperiod的工作参数。

以下是案例程序，main主流程部分的代码：

```
print('\n#1,设置 BT 量化回测程序入口')
```

```
cerebro = bt.Cerebro()

#
print('\n#2,设置 BT 回测初始参数：起始资金等')
dmoney0=100000.0
cerebro.broker.setcash(dmoney0)
dcash0=cerebro.broker.startingcash

#
#
print('\n\t#2-2,设置数据文件, 需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fdraised=rs0+xcod+'.csv'
print('\t@数据文件名：',fdraised)

#
print('\t 设置数据BT 回测运算：起始时间、结束时间')
print('\t 数据文件，可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t 格式为：标准 OHLC 格式，可以是日线、分时数据')
t0str,t9str='2018-10-01','2018-12-31'
data=tq.pools_get4fn(fdraised,t0str,t9str)

#
# 增加一个股票名称变量 xcod，可以在策略函数、绘图中使用
cerebro.adddata(data,name=xcod)
#
#
print('\n\t#2-3,添加 BT 量化回测程序对应的策略参数')
print('\n\t# 案例当中使用的是 MA 均线策略')
#cerebro.addstrategy(TQSta001,maperiod=5)
cerebro.addstrategy(TQSta001,maperiod=15,fgPrint=False)
#
print('\n\t#2-4,添加经纪人佣金，默认为：千一')
cerebro.broker.setcommission(commission=0.001)
#
```
