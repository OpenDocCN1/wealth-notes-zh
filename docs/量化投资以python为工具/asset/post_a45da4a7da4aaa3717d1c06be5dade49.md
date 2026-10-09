
从代码结果可以看出，单纯从中国银行股票来说，10 日简单均线交易策略的买卖点预测准确率较低。

## 30.8.2 双均线交叉捕捉中国银行股票的买卖点

双均线交叉策略的思想是利用长短期均线的相对关系，来识别价格趋势的变化。其策略制定过程有三个步骤。

- 先求出短期均线和长期均线；
- 当短期均线从下向上穿过长期均线时，释放出买入信号；
- 当短期均线从上向下降穿过长期均线时，释放出卖出信号。

再次提醒读者，短期和长期的时间跨度是因人而异的。

比如，选择 5 日简单移动平均线为短期线，30 日简单移动平均线为长期线。运用长短期均线交叉策略的思想，用 Python 捕捉长短期均线突破点，并且制定中国银行股票 2014 年 1 月到 2015 年 4 月份的买卖交易点。

```
# 计算 5 日 sma
# 计算 30 日 sma
In [10]: Ssma5=ma.smaCal(CBClose,5);
....: Lsma30=ma.smaCal(CBClose,30);
```

接下来，寻找买卖交易信号并制定交易执行日期。

```
In [11]: SLSignal=pd.Series(0, index=Lsma30.index)
....: for i in range(1,len(Lsma30)):
....: if all([Ssma5[i]>Lsma30[i], Ssma5[i-1]<Lsma30[i-1]]):
....: SLSignal[i]=1
....: elif all([Ssma5[i]<Lsma30[i], Ssma5[i-1]>Lsma30[i-1]]):
....: SLSignal[i]=-1
....:

In [12]: SLSignal[SLSignal==1]
Out[12]:
Date
2014-03-26 1
2014-05-13 1
2014-07-29 1
2014-09-05 1
2014-10-06 1
2014-10-31 1
2015-03-16 1
dtype: int64

In [13]: SLSignal[SLSignal==-1]
Out[13]:
Date
2014-03-03 -1
2014-05-02 -1
2014-06-27 -1
2014-08-29	-1
2014-09-23	-1
2014-10-08	-1
2015-02-03	-1
dtype: int64
```

```
In [14]: SLTrade=SLSignal.shift(1)

In [15]: Long=pd.Series(0,index=Lsma30.index)
...	Long[SLTrade==1]=1
...	CBRet=CBClose/CBClose.shift(1)-1
...	LongRet=(Long*CBRet).dropna()
...	winRate=len(LongRet[LongRet>0])/len(LongRet[LongRet!=0] )
...	winRate
Out[15]: 0.5
# 以 winRate 的值为 0.5 可以得出，
# 双均线交叉捕捉买入点的预测准确率为50%。

# 计算卖出点的预测获胜率
In [16]: Short=	pd.Series(0,index=Lsma30.index)
...	Short[SLTrade==-1]=1
...	ShortRet=(Short*CBRet).dropna()
...	winSrate=len(ShortRet[ShortRet>0])/len(ShortRet[ShortRet!=0])
...	winSrate
Out[16]: 0.4
# 双均线交叉捕捉卖出点的预测准确率为 40%

# 计算所有买卖点的预测获胜率
In [17]: SLtradeRet=(SLTrade*CBRet).dropna()
...	winRate=	len(SLtradeRet[SLtradeRet>0])/len(
...	SLtradeRet[SLtradeRet!=0])
...	winRate
Out[17]: 0.44444444444444444
```

根据上述双均线交易策略发现的买卖点日期，制定三种交易策略，即只进行买入点交易、卖出交易，以及既有买入交易又有卖出交易，对比分析这三种交易策略的绩效表现，结果如图 30.8 所示。

```
In [18]: cumLong=np.cumprod(1+LongRet)-1
...	cumShort=np.cumprod(1+ShortRet)-1
...	cumSLtrade=np.cumprod(1+SLtradeRet)-1

In [19]: plt.rcParams['axes.unicode_minus'] = False
...	plt.plot(cumSLtrade,label="cumSLtrade",color='b')
...	plt.plot(cumLong,	label="cumStock",\
...			color='r',linestyle='dashdot')
...	plt.plot(cumShort,label="cumTrade",\
...			color='-',linestyle=':')
...	plt.title("中国银行股票双均线交易")
...	plt.legend(loc='lower')
Out[19]: <matplotlib.legend.Legend at 0x1e8fb011e48>
```

![](img/0de14a685304f8aa267f6492145864ac_482_0.png)

图 30.8 长期均线交易绩效表现

## 30.9 异同移动平均线（MACD）

异同均线（Moving Average Convergence / Divergence）是由指数均线演变过来，由 Gerald Appel 在 20 世纪 70 年代提出，是股票分析中一种常见的指标。MACD 指标由两线一柱组合起来形成，快速线为 DIF，慢速线为 DEA，柱状图为 MACD。快速线 DIF 一般由 12 日指数加权移动平均值减掉 26 日指数加权移动平均值得到，慢速线 DEA 是 DIF 的 9 日指数加权移动平均值。柱状图 MACD 由快速线 DIF 与慢速线 DEA 作差得到。MACD 指标可以反映出股票近期价格走势的能量和变化强度，通过分析快慢速线和柱状图来把握股票的买入和卖出点。一般默认 MACD 的求值参数为 12、26 和 9，在投资实践中，可以对这些参数进行优化。

### 30.9.1 MACD 的求值过程

- 计算离差值 DIF，DIF 的计算方式为 12 日指数移动平均值 $EMA_{(close, 12)}$ 减去 26 日指数移动平均值 $EMA_{(close, 26)}$ ，数学公式表达为：

$$DIF= EMA_{(close, 12)}-EMA_{(close, 26)}$$

以中国银行股票交易数据为例，用 Python 来计算离差值 DIF。

```
In [1]: DIF=ma.ewmaCal(CBClose,12,2/(1+12))\
...:       -ma.ewmaCal(CBClose,26,2/(1+26))
...: DIF.tail(n=3)
Out[1]:
Date
2015-04-28    0.150903
2015-04-29    0.155585
2015-04-30     0.147109

dtype: float64
```

- 计算 DEA 的值。求出离差值 DIF 以后，一般选取 9 日为时间跨度，求离差值 DIF 的 9 日指数移动平均值 $EMA_{(DIF, 9)}$，这个平均值也就是 MACD 的衡量指标。用 DEA 或者 DEF 来表示这一均值，即：

$$\text{DEA} = EMA_{(DIF, 9)}$$

另外，由 DEA 画出来的线也被称作信号线。

```
In [2]: DEA=ma.ewmaCal(DIF,9,2/(1+9))
          ...: DEA.tail()

Out[2]:

Date
2015-04-24     0.133966
2015-04-27     0.133887
2015-04-28     0.137290
2015-04-29     0.140949
2015-04-30     0.142181
dtype: float64
```

- 计算 MACD 的值:

$$\text{MACD} = DIF - DEA$$

```
In [3]: MACD=DIF-DEA
          ...: MACD.tail(n=3)

Out[3]:

Date
2015-04-28     0.013613
2015-04-29     0.014636
2015-04-30     0.004928
dtype: float64
```

- 绘制两条信号线 DIF 和 DEA 与一个柱状图 MACD。如图 30.9 所示。

```
In [4]: plt.rcParams['font.sans-serif'] = ['SimHei']
          ...: plt.subplot(211)
          ...: plt.plot(DIF['2015'],\
          ...:           label='DIF',color='k')
          ...: plt.plot(DEA['2015'], label='DEA',\
          ...:           color='b',linestyle='dashed')
          ...: plt.title("沪深300 DIF 与 DEA")
          ...: plt.legend()
          ...: plt.subplot(212)
          ...: plt.bar(left=MACD['2015'].index,\
          ...:         height=MACD['2015'],\
          ...:         label='MACD',color='r')
          ...: plt.legend()

Out[4]: <matplotlib.legend.Legend at 0x1e8fb2e1b70>
```

![](img/0de14a685304f8aa267f6492145864ac_484_0.png)

### 30.9.2 异同均线 (MACD) 捕捉中国银行股票的买卖点

MACD 是投资实战中被投资者广泛使用的指标，由双移动平均线发展变化而来，再引入一个新的指标 (DIF) 来刻画短期和长期移动平均线的聚合与偏离，并对双均线的差离再次平均，得到一条信号线 (DEA)。如图 30.10 所示。

![](img/0de14a685304f8aa267f6492145864ac_484_1.png)

```
In [5]: macddata=pd.DataFrame()
 ...: macddata['DIF']=DIF['2015']
 ...: macddata['DEA']=DEA['2015']
 ...: macddata['MACD'] = MACD['2015']

candle 模块是本书自定模块，在前面章节有介绍
In [6]: import candle

....: candle.candleLinePlots(ChinaBank['2015'],\
....: candleTitle='中国银行 2015年 3月 9日',
....: splitFigures=True, Data=macddata,\
....: ylabel='MACD')
```

我们阐述一些常用的 MACD 交易思想。图 30.10 下方 MACD 图中的实线表示 DIF，虚线表示 DEA 信号线。

- 当 DIF 和 DEA 都在零刻度线上方时，表明市场可能是多头行情；反之，当 DIF 和 DEA 都在零刻度下方时，表明市场可能处于空头行情。在投资实战中，“零上双金叉”策略广为人们使用。DIF 和 DEA 都在零刻度线上方，在一段时间内，DIF 先上穿 DEA 线，不久 DIF 下跌到 DEA 线的下方，然后 DIF 又上穿 DEA 线，此时，说明股票价格上升趋势较强，市场处于上涨行情中。
- 当 DIF 下穿信号线 DEA 时，释放出买入信号；当 DIF 向上穿过头信号线 DEA 时，释放出卖出信号。在中国银行 2015 年交易行情中，DIF 几乎都处于 DEA 的上方，两条线相交的次数较少，释放出的买卖信号也较少。在 2015 年 3 月 4 日附近，DIF（实线）从上向下穿越 DEA（虚线），DIF 和 DEA 的取值都小于 0；观察 K 线图，可以推测市场可能处于下跌行情。在 3 月 9 日附近，两条线都呈现上升趋势，DIF 从下向上穿越 DEA，释放出市场慢慢走强的信号。观察 K 线图可知，从 3 月 9 日以后，市场大体上处于上升行情中。
- MACD 中的柱形图表示 DIF 与 DEA 的差值。柱形图的高低表示 DIF 与 DEA 差值的大小。柱形图在零刻度附近时，释放出买卖信号。柱形图在零刻度线上方，表示 DIF 大于 DEA，市场走势较强；柱形图在零刻度下方，表示 DIF 小于 DEA，市场走势较弱。
- 在投资实战中，柱形图的形态也是投资者关注的一大焦点，MACD 指标的形态分析方法可以套用一般的形态分析理论与方法。MACD 柱形图高低的变化与价格线走势的背离情况也可以被解读为交易信号。

在一般技术分析中，MACD 指标的分析方式不可尽述，优化 MACD 的参数也可以衍生出更多的分析方式。以 DIF 与 DEA 线的交叉与背离为例，用 Python 刻画这一策略的情景并自动捕捉买卖点。

DIF 与 DEA 线的交叉与背离这一策略的分析过程包括如下 3 个步骤。

- 先求出 DIF（差离值，快线）、DEA（信号线，慢线）的值；
- DIF、DEA 均为正，DIF 向上突破 DEA，买入信号；
- DIF、DEA 均为负，DIF 向下跌破 DEA，卖出信号。

DIF 和 DEA 两条线向上穿越或者向下穿越与短期均线 sma5 和长期均线 sma30 的交叉的 Python 代码编写思路大致相同。

```
In [7]: macdSignal=pd.Series(0, index=DIF.index[:])
     ...: for i in range(1, len(DIF)):
     ...:     if all((DIF[i]>DEA[i])>0.0, DIF[i-1]<DEA[i-1]):
     ...:         macdSignal[i]=1
     ...:     elif all((DIF[i]>DEA[i])<0.0, DIF[i-1]>DEA[i-1]):
     ...:         macdSignal[i]=-1
     ...:
```

```
In [8]: macdSignal.tail()
Out[8]:
Date
2015-04-24    0
2015-04-27    0
2015-04-28    1
2015-04-29    0
2015-04-30    0
dtype: int64
```

```
In [9]: macdTrade=macdSignal.shift(1)
```

```
In [10]: CBRet=CBClose/CBClose.shift(1)-1
     ...: macdRet=(CBRet*macdTrade).dropna()
     ...: macdRet[macdRet==0]=0
     ...: macdWinRate=len(macdRet[macdRet>0])/len(macdRet[macdRet!=0])
     ...: macdWinRate
Out[10]: 0.5
```

## 30.10 多种均线指标综合运用模拟实测

通过前面小节的叙述可知，简单移动平均线、双均线交叉和异同移动平均线均可用于捕捉买卖点。现在将这 3 个交易策略综合在一起，共同捕捉买卖点。尝试运用多种指标捕捉买卖点，一方面可以捕捉到更多的买卖点[^1]；另一方面，在买卖点信号方面，不同指标释放的交易信号甚至可能会相反。比如，简单移动平均线是在某一交易日期放出买入信号，而双均线交叉策略在当期释放出卖出信号。多种指标分析有助于我们多方面研判市场行情，交易决策的制定会更加谨慎，进而有可能提高交易策略的获胜率。

在实际交易时，我们会有交易资金、购买股票的份额等方面的考虑。在综合运用多种均线指标制定交易策略时，除了捕捉买卖点以外，还可以把交易资金、购买股票的份额等实际因素考虑进去。在以下交易策略的制定上，我们考虑下面的三大问题。

- 计算指标，制定买卖交易信号。
- 设定初始资金，有交易信号时，进行买卖交易。
- 投资交易的回测与评价。

[^1]: 注意，不同指标捕捉的买卖点信号不一定一致；有信号也并不代表我们可以不假思索就据以买卖。

接下来，计算3种均线指标、刻画交易思想、捕捉买卖点信号并实践交易模型。

- 首先，将上述3种策略捕捉的买卖点信号综合在一起，计算新的买卖交易信号；

```
# 合并交易信号
In [1]: AllSignal=SmaSignal+SLSignal+macdSignal
```

```
In [2]: for i in AllSignal.index:          
...:  if AllSignal[i]>1:           
...:       AllSignal[i]=1           
...:  elif AllSignal[i]<-1:          
...:       AllSignal[i]=-1           
...:  else:           
...:       AllSignal[i]=0           
...:
```

```
In [3]: AllSignal[AllSignal==1]
Out[3]:
Date
2014-05-13    1
2015-01-21    1
dtype: int64
```

```
In [4]: AllSignal[AllSignal==-1]
Out[4]:
Date
2014-06-27    -1
2014-10-08    -1
2015-03-03    -1
dtype: int64
```

```
In [5]: tradSig=AllSignal.shift(1).dropna()
```

- 设定初始资产20,000元，根据买卖点讯号，进行中国银行模拟交易。

```
In [6]: Close=CBClose[-n:]
...:  asset=pd.Series(0.0,index=Close.index)
...:  cash=pd.Series(0.0,index=Close.index)
...:  share=pd.Series(0,index=Close.index)
```

```
# 当价格连续两天上升且交易信号没有显示卖出时，
# 第一次开账户持有股票
```

```
In [7]: entry=3
...:  cash[entry]=20000
...:  while entry<len(CBClose):
...:       cash[entry]=cash[entry-1]
...:       if all((CBClose[entry-1]>=CBClose[entry-2],\
...:                CBClose[entry-2]>=CBClose[entry-3],\
...:                AllSignal[entry-1]!=-1]):
...:           share[entry]=1000
...:           cash[entry]= cash[entry]-1000*CBClose[entry]
...:           break
...:       entry+=1
...:  
```
