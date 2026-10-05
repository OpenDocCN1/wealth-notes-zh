
![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_342_0.png)

## 图15-2 Yahoo（雅虎）金融数据源子模块关系属性示意图

由图15-2可以看出，用户自定义数据设计主要相关的子模块库有 Feed、Store等。

国外的股票外汇等金融市场与国内的不同。因为国内金融政策的限制，很多金融数据API接口都没有开放，个别交易所对大客户采用专用软件的方式，提供live-data实时金融数据接口。

国内在金融数据方面目前是半开放模式，且数据源和下单环节没有完全开放，大部分免费数据接口只支持到日线级别的交易。

对于个人用户来说，金融数据源看起来是一个瓶颈，但其实影响并不大。国内的交易所采用T+1的交易模式，不支持日内交易。由于个人用户的资金有限，所以绝大部分个人用户采用隔日交易模式，而日线交易数据有很多免费的金融模块库，对于交易机构以及期货交易者来说，可以通过购买商务接口，来获取交易数据。所以，当进行实盘操作时，在金融数据源方面的影响不大。

## 15.3 数据共性

前面从交易角度分析了模拟盘和实盘交易的区别。下面看看数据的共性。

量化软件的数据格式，通常都是OHLC格式的。OHLC是金融行业的标准数据格式，支持日线分时数据和tick数据。

不管是模拟盘操作，还是实盘操作，也不运行什么金融项目（股票、期货、外汇、数字货币等），其操作逻辑都是相同的。

很多人认为，量化交易方式只针对股票这一种产品，其实不是的，所有的金融产品（股票、期货、外汇、石油、贵重金属等）都可以通过量化交易的方式进行操作。只要是有稳定的、基于时序的历史交易数据的产品，都可以通过BackTrader这种专业量化软件进行回测分析。进行量化分析的前提是要有数据，只要数据是稳定的、标准的就可以了。

量化程序输入数据的格式主要有以下三种：

- CSV格式，金融行业标准数据交换文件格式。
- dBase数据库格式。
- stream数据流格式。

（1）CSV格式。

CSV是BackTrader量化软件当中最基本的数据格式之一，如图15-3所示是CSVFeedBase类函数关系属性示意图。

## 图15-3 CSVFeedBase类函数关系属性示意图

由图15-3可以看出，该函数属于Feed数据子模块，其主要相关的子模块库有：Feed、Cerebro、Line、Indicators、Store等。

CSV格式的数据文件，在本质上是一个txt文本文件，便于查看和编辑。txt文本格式是很重要的属性，大部分用户往往会忽略这一点，而会基于容量等原因采用一些其他的非文本格式。

采用非文本格式，看起来数据量会小一点，但在实际使用时，特别是在不同程序之间进行数据交换时，过程会非常复杂，有时甚至还需要

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_344_0.png)

专门的工具软件进行读取、转换。

目前，绝大部分的交换数据基于JSON、XML等文本格式，主要原因在于：用较低的机器成本，来替代较高的人工成本。现在存储设备（比如硬盘、SSD固态硬盘等）的价格非常低廉，而人工成本却越来越高。

CSV格式是金融行业的数据交换标准格式，使用这种格式所存储的文件便于生成、读取。第三方的软件编程也很容易生成和读取CSV格式的数据，方便不同程序之间进行数据交换。

（2）dBase数据库格式。

使用dBase数据库格式的数据成本很高，主要体现在以下几个方面：

- 设备成本，通常需要购买专业的数据服务器。如果采用计算机后台程序模式，则会影响其他程序的运行效率。
- 时间成本，特别是金融行业，一线职员的时间成本很高。
- 维护成本，通常还需要专业技术人员对数据库进行维护。

（3）stream数据流格式。

stream数据流格式是目前使用较多的实时数据模式，需要专业编程技术，使用起来很方便。实时stream数据流主要用于Web在线、实时数据图表模式。

有些大型交易所提供同步的实时交易数据，用户可以通过接口同步显示现实的交易数据，进行实时的可视化分析。

## 15.4 数据区别

下面看看模拟盘交易和实盘交易在数据方面的区别。

模拟盘交易所使用的数据一般是历史数据、随机数据或者仿真数据。在实盘交易中进行量化回测时，通常使用历史数据，但历史数据存在偏差的问题。

理论上，进行模拟盘交易时最好使用仿真数据，以防止出现这种偏差。

仿真数据源来自真实的历史数据，只是增加了一些简单的数据变化，这样可以有效防止大家对历史数据产生先入为主的问题，减少相关的干扰。

在进行实盘交易时，每次交易都要使用最新的实时数据，如果使用过去的历史数据，就肯定会出现交易失误。

实盘交易对于数据源而言，有两个要求：

- （1）使用真实数据。
- （2）使用实时数据。

各个交易所对实时数据的使用都有所不同，如图15-4所示是BackTrader量化软件内置的Interactive Brokers（简称为IB）交易所的IBData实时数据模块关系属性示意图。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_346_0.png)

## 图15-4 IBData实时数据模块关系属性示意图

由图15-4可以看出，其主要相关的子模块库有Cerebro、Feed等。

## 15.5 案例：模拟盘的参数设置

本节案例程序文件名是：btr_e029sim.py，介绍模拟盘的参数设置。以下是案例程序中main主流程部分的代码：

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
print('\n\t#2-2,设置数据文件,要按时间字段正序排序')
```

```
rs0='data/stk/'
#rs0='/tqdat/day/stk/'
#rs0='/tqdat/min/m15/stk/'
xcod='002046'
fdatt=rs0+xcod+'.csv'
print('\\t@数据文件名：', fdat)

#
print('\\t 设置数据 BT 回测运算：起始时间、结束时间')
print('\\t 数据文件，可以是股票期货、外汇黄金、数字货币等交易数据')
print('\\t 格式为：标准 OHLC 格式，可以是日线、分时数据')
#t0str,t9str='2010-01-01','2018-12-31'
#t0str,t9str='2018-01-01','2018-12-31'
t0str,t9str='2018-10-01','2018-12-31'
t0str,t9str='2018-10-01',''
data=tq.pools_get4fn(fdat,t0str,t9str)

#
# 增加一个股票名称变量 xcod，可以在策略函数、绘图中使用
cerebro.adddata(data,name=xcod)
#
#
print('\\n\\t#2-3,添加 BT 量化回测程序对应的策略参数')
print('\\n\\t# 案例当中使用的是 MA 均线策略')
cerebro.addstrategy(ma,period=30)
#
print('\\n\\t#2-4,添加 Broker 经纪人佣金，默认为：千一')
cerebro.broker.setcommission(commission=0.001)
#
print('\\n\\t#2-5,设置每手交易数目为：10，不是默认值：1 手')
cerebro.addsizer(bt.sizers.FixedSize, stake=10)

print('\\n\\t#2-5,设置 addanalyzer 分析参数')

cerebro.addanalyzer(SQN)
#
```

```
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
    print('\t ROI 投资回报率 Return on investment: %.2f %%' % kret)
    
    #
    #--------
    print('\n#8,Analyzer 分析 BT 量化回测数据')
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
```

```
#
print('\t 夏普指数 SharpeRatio : ', dsharp)
print('\t 最大回撤周期 max_drowdown_len : ', max_drowdown_len)
print('\t 最大回撤 max_drowdown : ', max_drowdown)
print('\t 最大回撤(资金)max_drowdown_money : ',
    max_drowdown_money)

#---------
#
print('\n#9,绘制 BT 量化分析图形')
cerebro.plot()

#---------
#
print('\n zok')
```

案例主流程使用的是单只股票，大家可以通过修改股票代码的方式进行设置：

```
rs0='data/stk/'
xcod='002046'
fdal=rs0+xcod+'.csv'
```

本案例数据目录data使用的是相对目录，位于案例目录data子目录中。采用相对目录，可以把案例数据直接放在程序目录下面，案例程序无须集成数据包，这样可以方便大家学习和复制案例程序，也不会出现数据缺失的错误问题。

无论是模拟盘交易还是实盘交易，都需要使用最新的交易数据，但使用相对数据目录就存在一个潜在问题：使用的是历史数据，在进行模拟盘交易时无法更新为最新的交易数据。此时，可以通过手动模式，从数据包复制最新的交易数据，但是不要复制全部的股票代码数据，因为文件数量太多，而且案例也没有涉及，一般只需要复制与案例相关的数据即可。

在进行实盘交易时，对于回测时间周期，一般设置为30天。所以回测起始时间，一般为30天以前。

如果回测起始时间和回测终止时间都设置为空字符串，则表示对全部数据进行回测分析。

运行程序：

```
#3,调用 BT 回测入口程序, 开始执行 run 量化回测运算
2018-11-16T00:00:00, 设置买单 BUY CREATE, 6.54, name : 002046,tim:0.00 s
2018-11-19T00:00:00, 卖单执行 SELL EXECUTED,成交价: 6.54,小计 Cost: 65.40,佣金 Comm:0.07, tim:0.00 s
2018-12-19T00:00:00, 设置买单 SELL CREATE, 6.56, name : 002046,tim:0.00 s
2018-12-20T00:00:00, 卖单执行 SELL EXECUTED,成交价:6.54,小计 Cost: 65.40,佣金 Comm: 0.07, tim:0.00 s
2018-12-28T00:00:00, (忽略参数 Period=30) ,最终资产总值: 99999.87, tim:0.02 s
```

在以上输出信息中，9月28日以后的数据没有输出信息，因为10月1日是国庆长假，没有交易数据。

再看看其他测试数据：

```
#4,完成 BT 量化回测运算
    起始资金 Starting Portfolio Value: 100000.00
    资产总值 Final Portfolio Value: 99999.87
    利润总额: -0.13,
    ROI投资回报率 Return on investment: -0.00 %
```

本案例中，10万元本金只亏损9元，案例中的MA均线策略没有亏损，也没有盈利。

本书内容到此就全部结束了。通过本书的学习，大家对BackTrader量化软件已经有了一个初步了解。本书配套的案例程序都有案例源码，且源码内置中文注解，全部深入分组一级，对于初学者来说，学习起来特别方便。

## · 好书分享 ·

## 零基础搭建
韧性量化交易系统

## 零基础学MQL
基于EA的自动化交易编程

## 程序自动化交易
投资策略解析

## BackTrader
量化交易案例图解

## 本书特点

- ◆ IT零起点：不需要任何编程基础，就能看懂本书。
- ◆ 投资零起点：无须购买任何专业软件，本书配套的zwPython软件包，采用开源模式，提供免费的工业级数据分析平台。
- ◆ 理财零起点：采用通俗易懂的语言，配合大量专业图表和实盘操作案例，让你轻松掌握各种量化投资策略。
- ◆ 数学零起点：没有复杂的数学公式，只有最基本的加、减、乘、除，你可以轻轻松看懂全书。

责任编辑：黄爱辉
封面设计：李 玲

上架建议：金融/量化交易

ISBN 978-7-121-39705-9
定价：79.00元
