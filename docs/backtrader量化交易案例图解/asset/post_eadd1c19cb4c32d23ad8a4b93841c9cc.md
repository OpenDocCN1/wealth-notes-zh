
```
def __init__(self,vdict={}):
    # 默认数据，一般使用股票池当中下标为 0 的股票
    # 通常使用 Close 收盘价作为主要分析数据字段
    self.dataclose = self.datas[0].close
    #
    #
    if len(vdict)>0:
```

```
self.p.period=int(vdict.get('period'))
```

通过多组测试，决定采用dict字典格式传递参数变量。BackTrader量化软件本身的变量参数也是主要通过dict字典格式传递的。

在案例init函数中，还应增加一个tim0wrk变量，用于记录量化程序回测运行的启动时间：

```
self.tim0wrk=arrow.now()
```

程序采用的是arrow时间模块库，运行后有多组输出数据，对应不同的周期参数：

```
#3-1,单参数自动寻优测试
2018-12-28, (策略参数 Period=5), 最终资产总值: 999600.75, tim: 0.18 s
2018-12-28, (策略参数 Period=10), 最终资产总值: 999707.21, tim: 0.16 s
2018-12-28, (策略参数 Period=15), 最终资产总值: 999797.01, tim: 0.16 s
2018-12-28, (策略参数 Period=20), 最终资产总值: 999899.38, tim: 0.16 s
2018-12-28, (策略参数 Period=25), 最终资产总值: 999886.20, tim: 0.15 s
2018-12-28, (策略参数 Period=30), 最终资产总值: 999930.76, tim: 0.15 s
2018-12-28, (策略参数 Period=35), 最终资产总值: 999927.06, tim: 0.15 s
2018-12-28, (策略参数 Period=40), 最终资产总值: 999843.24, tim: 0.15 s
2018-12-28, (策略参数 Period=45), 最终资产总值: 999746.86, tim: 0.16 s
```

本案例使用了股票池数据：

```
print('\t设置指数池，股票池代码')
baslst=['000001'] #上证，深证，A股，B股

syblst=['002046','600663','600809','600648','600600','600638']
通过 TopQuant 量化函数工具包，BackTrader 很容易支持股票池，以及指数池
#
print('\t读取股票池、指数池数据')
#tq.pools_get4flst(qx,rbas0,baslst,tim0str,tim9str,fgInx=True)
tq.pools_get4flst(qx,rsyb0,syblst,tim0str,tim9str,fgInx=False)
```

本节案例没有使用指数数据，这里对指数数据进行了屏蔽。系统可以读取股票交易数据，也可以读取期货、外汇的交易数据，这只是数据源的切换问题。

BackTrader量化软件对数据源的支持非常灵活，只要是标准的OHLC金融交易数据或者ask-bid数据、tick数据，就都可以通过BackTrader量化软件进行回测分析。

下面看一看main主流程中MA均线策略的代码，其中period变量是单变量MA均线策略的时间周期参数：

```
# 创建一个最简单的 MA 均线策略类
class ma(bt.Strategy):
    # 定义 MA 均线策略的周期参数变量，默认值是 15
    # 增加一个 log 打印开关变量 fgPrint，默认是关闭
    params = (('period', 15),('fgPrint', False), )
```

具体的参数优化操作，是通过主流程当中bt_opt参数优化函数完成的：

```
print('\n#3-1,单参数自动寻优测试')
d10=list(range(5, 50,5))
sgnlst=['period']
x100=tq.bt_opt(qx,ma,[d10],sgnlst)
```

注意，以上代码当中的参数值变量d10其实是一个list参数组，每一组数据都代表一轮的测试数据，类似于神经网络的echo的一次迭代计算。

d10变量中的数据是使用Python常用的range函数生成的一组数据，数据间隔是5。

程序运算速度非常快，在案例中，股票池有6只股票，都是使用2018年全年的日线数据，每轮都需要计算6只股票全年的数据，每轮平均回测的时间大约是0.2秒。换算为单只股票一年日线数据，其回测时间约0.03秒，大约1秒钟可以回测一百万条交易数据。

BackTrader量化软件内部经过了高度优化，底层采用Pandas矢量化运算模式，运算速度很快。

在案例的输出信息中，有当前的参数值及运行时间。

```
#3-1,单参数自动寻优回测
2018-12-28, (策略参数 Period=5), 最终资产总值: 999600.75, tim: 0.18 s
2018-12-28, (策略参数 Period=10), 最终资产总值: 999707.21, tim: 0.16 s
```

以上数据源自策略当中的stop函数：

```
def stop(self):
# 新增一个 stop 策略完成函数
# 用于输出执行后数据
self.log('(策略参数 Period=%2d), 最终资产总值: %.2f' %
(self.params.period, self.Broker.getvalue()), fgPrint=True)
```

大家可以通过spyder或者vs-code的函数导航面板，快速定位到stop函数代码部分。

在一般的策略编程案例当中，只有init初始化函数、next交易函数、notify_order订单信息处理函数等。

在本案例中，stop函数增加了一个输出语句，输出当前的策略参数，以及最终资产总值等信息。

这样的设计便于下一步分析，以及对参数进行筛选。通常，资产总值最高的参数是最好的。

这种模式也不是绝对正确的，例如在大盘不好的极端情况下，某些策略可能根本没有交易行为，一直是初始资金，所以没有亏损。此外，交易次数和回测的时间周期也有关系，回测周期长，交易次数就多。

再看一看优化函数的返回数据，返回数据也是dict字典格式，按最终资产总值进行了排序处理：

```
@x {'30': 999930.756, '35': 999927.065, '20': 999899.38, '25': 999886.195, '40': 999843.239, '15': 999797.015, '45': 999746.858, '10': 999707.21, '5': 999600.746}
```

## 14.3 BackTrader内置优化函数

BackTrader网站内置的optstrategy（简称为opt）参数优化函数，使用相对更加简单，但没有TopQuant极宽优化版本灵活。TopQuant极宽优化版本可以批量传递多组参数，甚至可以传递不同的策略。

如图14-1所示是BackTrader量化软件中，optstrategy参数优化函数关系属性示意图。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_332_0.png)

图14-1 optstrategy参数优化函数关系属性示意图

由图14-1可以看出，该函数属于cerebro量化回测类的类函数之一。

在BackTrader量化软件中，通常采用addstrategy函数和以下代码添加策略：

```
cerebro.addstrategy(StrategyClass, *args, **kwargs)
```

其中，“*args, **kwargs”代表对应的策略参数，这是Python的动态参数模式。

在对策略进行优化时，使用optstrategy策略优化函数：

```
cerebro.optstrategy(StrategyClass, *args, **kwargs)
```

策略优化函数中的“*args, **kwargs”策略参数变量并非单个的数值，而是一个list列表类型，表示参数的测试范围。

以下是一个MACD策略的优化案例代码：

```
cerebro.optstrategy(
    macd_sta,
    smaperiod=xrange(5, 40),
    macdperiod1=xrange(12, 20),
    macdperiod2=xrange(26, 30),
    macdperiod3=xrange(9, 15),
)
```

其中的参数变量如下：

- macd_sta，是macd_sta策略名称。
- smaperiod，是MA均线时间周期参数。
- macdperiod1、macdperiod2、macdperiod3，是MACD策略参数。

## 14.4 演示案例：多参数自动寻优

下面看看多参数的优化案例。注意：本案例属于演示案例，没有提供完整的案例源码，只提供核心代码和案例结果数据供大家参考。

本节案例的程序文件名是：btr_f031opt2x.py。以下是案例程序中 main主流程部分的代码：

```
print('\n#1,设置 BT 量化回测全局变量 qx, 并初始化')
qx=tq.tq_init('TQ01',cash0=1000000.0)

#
print('\n\t#2-1,设置数据文件, 需要按时间字段正序排序')
#rs0='/TQDat/day//'
rs0='data/'
rsyb0,rbas0=rs0+'stk/',rs0+'inx/'

#
print('\t 设置指数池, 股票池代码')
baslst=['000001'] #上证
syblst=['002046','600663','600809','600648','600600','600638']
#syblst=['000001','002046']
#
print('\t 设置数据 BT 回测运算: 起始时间、结束时间')
print('\t 数据文件, 可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t 格式为: 标准 OHLC 格式, 可以是日线、分时数据')
tim0str,tim9str='2018-01-01','2018-12-31'
```

```
#
print('\t 读取股票池、指数池数据')
#tq.pools_get4flst(qx,rbas0,baslst,tim0str,tim9str,fgInx=True)
tq.pools_get4flst(qx,rsyb0,syblst,tim0str,tim9str,fgInx=False)

#------------
print('\n#2-2,设置 BT 参数, anzMod 为分析模式, 默认0')
tq.bt_set(qx,anzMod=1)
#
#
#--------
print('\n#3,调用 BT 回测入口程序, 执行 opt 参数优化函数')
#原版 optstrategy 策略优化函数 API 接口
##cerebro.optstrategy(MyStrategy, myparam1=range(10, 20))
#
print('\n#3-1,单参数自动寻优测试')
#d10=list(range(5, 50,5))
#sgnlst=['period']
#x100=tq.bt_opt(qx,ma,[d10],sgnlst)
#xxx
#
print('\n#3-2,多参数自动寻优测试')
d10=list(range(5, 35,5))
d20=list(range(10, 190,20))
sgnlst=['nfast','nslow']
vlst=[d10,d20]
x100=tq.bt_opt(qx,ma_cross,vlst,sgnlst)

#------------
print('\n@x.end#9, 没有plot 绘图, analyzer 分析部分')
#qx.plot(figsize=(1500,500))

print('\nzok')
```

在案例中的双参数策略——MA-cross均线交叉策略中，使用nfast、nslow两个变量分别代表长周期、短周期的时间周期参数。

```
class ma_cross(bt.Strategy):
    params = (('nfast', 10), ('nslow', 30),('fgPrint', False),)
```

在init函数中，通过迭代计算，把计算结果保存在buysig数组变量中，这是一个准买卖信号：

```
def __init__(self,vdict={}):
    # To keep track of pending orders and buy price/commission
    self.order = None
    self.buyprice = None
    self.buycomm = None
    self.tim0wrk=arrow.now()
    #
    #self.dataclose = self.datas[0].close
    #print('@@vdict',vdict)
    #
    if len(vdict)>0:
        self.p.nfast=int(vdict.get('nfast'))
        self.p.nslow=int(vdict.get('nslow'))
    #
    sma_fast,sma_slow,self.buysig={},{}, {}
    for xc,xdat in enumerate(self.datas):
        sma_fast[xc] =bt.ind.SMA(xdat,period=self.p.nfast)
        #sma_fast[xc] =bt.ind.SMA(xdat,period=self.p.nfast)
        sma_slow[xc] =bt.ind.SMA(xdat,period=self.p.nslow)
        self.buysig[xc] = bt.ind.CrossOver(sma_fast[xc],
            sma_slow[xc])
```

在stop结束函数部分，输出了两个参数变量数值：

```
def stop(self):
    self.log('策略参数 nfast= %2d, nslow= %2d, 资产总值: %.2f' %
        (self.p.nfast,self.p.nslow, self.Broker.getvalue()), fgPrint=True)
```

注意，在输出信息中，对应的参数名称也是不同的：

```
2018-12-28, 策略参数 nfast= 30, nslow= 130, 资产总值: 999896.05, tim:
0.20 s

2018-12-28, 策略参数 nfast=30, nslow=170, 资产总值: 1000000.00, tim:
0.20 s
```

在单参数MA均线策略案例中，参数名称是period。在本案例中，变量名称分别是nfast、nslow。

再看输出数据，当nfast=nslow时，资产总值都是100万元：

```
2018-12-28, 策略参数 nfast=10, nslow=10, 资产总值: 1000000.00, tim: 0.20 s
```

因为两根均线周期相同时没有交叉点，此时就没有交易产生，属于空交易现象。空交易现象无法评估策略的好坏。

空交易现象属于量化程序的一个bug，需要在stop函数中增加过滤代码。当遇到复杂策略，具有多组参数，且输出信息较多时，这类bug就很难被察觉。

所以笔者个人认为，人工操作是量化交易的最后一道防火墙。因为很多被计算机认为合理的策略，在进行人工操作时很容易就能发现问题所在。

量化策略一定要通过长期的测试，以防止意外情况发生，量化程序中的海量数据计算、参数优化，都必须通过计算机进行，但最后的决策还是由人工操作的。

在案例中，最优的参数组合是：'25_150'，只亏损91元。第二对组合：'5_170'，亏损92元，具体代码如下：

```
@x {'25_150': 999909.058, '5_170': 999908.501, '20_150': 999908.057,
'30_110': 999903.052, ....., '5_10': 996952.259}
```

在以上输出信息中，用下画线分隔多组数据，这样可以尽量简化程序结构，否则就要使用多维字典模式。

# 第15章 模拟盘/实盘操作

本章主要内容如下。

- 模拟盘交易和实盘交易的区别。
- 实盘数据和交易接口。
- 数据共性。
- 数据区别。
- 案例：模拟盘的参数设置。

# 15.1 模拟盘交易和实盘交易的区别

在量化交易中，模拟交易和实盘交易存在一定的区别，其中最大的区别在于有没有真实资金的投入。通过量化程序，按分析的数据进行交易，如果投入了实际资金，就是实盘交易；如果没有投入实际资金，就是模拟盘交易。

模拟盘交易类似于纸上谈兵，由系统推荐买卖订单，但用户并没有实际执行，只是记录下来作为参考。

如果在这个过程中，用户根据系统的推荐，使用实际资金进行了真实交易，那么模拟盘交易就变成了实盘交易。

从操盘者的角度而言，进行实盘和模拟盘交易最大的差别在于心态。从心理学角度而言，如果获得的东西比较廉价，就不会珍惜。

模拟盘交易和实盘交易的另外一个区别是对市场的干扰。例如，在 MACD指数刚问世时，是非常好的金融指标，但在其普及不久以后就失灵了。因为大家都在使用MACD指数进行市场交易，用的人数多了，结果就会反向干扰市场。

量化策略也是同样的道理，量化交易策略是实盘交易的核心商业秘密，如果大家都采用同样的量化策略进行市场交易，就会反向干扰市场。

# 15.2 实盘数据和交易接口

无论是模拟盘交易还是实盘交易，都离不开交易数据。笔者曾经说过：无数据，不量化。由此可见，数据在量化交易当中的重要性。

BackTrader的数据源函数主要位于Feed数据子模块中，如图15-1所示是Feed数据子模块关系属性示意图。

![](img/1c2fa9d9dc5f4ecf1f6a4ce7faab4913_341_0.png)

图15-1 Feed数据子模块关系属性示意图

由图15-1可以看出，其主要相关的子模块库有Broker、Feed等。

如图15-2所示是BackTrader量化程序中，内置的Yahoo雅虎金融数据源子模块关系属性示意图。
