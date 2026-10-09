
#设定字体类型，用于正确显示中文
plt.rcParams['font.sans-serif'] = ['SimHei']
plt.rcParams['axes.unicode_minus'] = False

def candlePlot(seriesData, title=""): 
    #设定日期格式
    Date=[date2num(date) for date in seriesData.index]
    seriesData.loc[:, 'Date']=Date
    
    #将DataFrame数据转换成List类型
    listData=[]
    for i in range(len(seriesData)):
        a=[seriesData.Date[i],\
           seriesData.Open[i],seriesData.High[i],\
           seriesData.Low[i],seriesData.Close[i]]
        listData.append(a)

# 设定绘图相关参数
ax = plt.subplot()
mondays = WeekdayLocator(MONDAY)
#日期格式为 '15-Mar-09' 形式
weekFormatter = DateFormatter('%y %b %d')
ax.xaxis.set_major_locator(mondays)
ax.xaxis.set_minor_locator(DayLocator())
ax.xaxis.set_major_formatter(weekFormatter)

#调用candlestick_ohlc函数
candlestick_ohlc(ax, listData, width=0.7,\
    colorup='r', colordown='g')
ax.set_title(title) #设定标题
#设定x轴日期显示角度
plt.setp(plt.gca().get_xticklabels(), \
    rotation=50, horizontalalignment='center')
return (plt.show())

## 27.3 Python 捕捉 K 线图的形态

观察股票的历史价格数据，从这些历史价格中寻找规律，进而预测股票未来价格。股票未来价格有两种情况：第一种是继续保持原来上涨或者下跌的趋势，即单边持续状态；第二种是双边整理状态，股票的价格走向在未来可能会出现反转，由下跌的趋势经过一些盘整调整逐渐转向上涨的趋势，或者由上涨的趋势通过调整转化成下跌的趋势。

将 1～5 个蜡烛图排列组合在一起，往往可以形成一些特殊“形态”。一般而言，K 线形态（Candlestick Pattern）可分为“持续形态（Continuation Patterns）”和“反转形态（Reversal Patterns）”。K 线图充分代表了股票的价格数据，分析 K 线图的形态有助于我们推测未来股价是继续保持原有的趋势还是会出现反转状态。接下来，介绍几种常见的 K 线图反转形态，以便读者初步了解 K 线形态。

### 27.3.1 Python捕捉“早晨之星”

#### 1. “早晨之星”与“黄昏之星”

“早晨之星”和“黄昏之星”是反转形态分析中较为常用的形态，它们分别释放出下跌趋势的反转信号与上涨趋势的反转信号。“早晨之星”又称“黎明之星”、“希望之星”，它一般会释放出下跌行情趋势反转的信号。如图27.4所示。

![](img/0de14a685304f8aa267f6492145864ac_424_0.png)

图27.4 “早晨之星”与“黄昏之星”的形态

“早晨之星”一般由连续3根蜡烛图组成。第1个蜡烛图具有较长的绿色实体（阴线）；紧接着第2个蜡烛图是十字星线；第3个蜡烛图有红色实体（阳线），且其实体长度一般要求大于等于第1个蜡烛图的实体长度的一半。此外，第2个蜡烛图的实体部分一般落在第1个和第3个蜡烛图实体的下方，即十字星的实体部分要低于前一期的收盘价和其后一期的开盘价。

如果在下跌行情中，出现“早晨之星”形态，则可以将其解释为股票的买方市场的力量在逐渐增大，股价可能有利好的趋势，股市可能由原来的下跌行情慢慢转变成上涨行情。

“黄昏之星”的形态与“早晨之星”形态相反，“黄昏之星”一般出现在上涨行情中。该形态由3个蜡烛图组成。第1个蜡烛图具有较长的红色实体，第2个蜡烛图为十字星线，第3个蜡烛图是阴线，且第3个蜡烛图的实体长度一般要大于等于第1个蜡烛图的红色实体长度的一半。同时，第2个蜡烛图的实体部分一般落在第1个和第3个蜡烛图实体的上方，即十字星的实体部分一般要高于前一期的收盘价和其后一期的开盘价。黄昏之星的出现预示着股票市场由买方市场逐渐转变成卖方市场，股票未来可能有下跌的趋势，说明股票的上涨行情有可能处于“黄昏阶段”。此处请注意，“早晨之星”和“黄昏之星”只是反转形态的一个信号，并不能作为反转形态的唯一依据。

#### 2. Python 捕捉上证综指 2012 年的“早晨之星”形态

在一般技术分析中，人们观察分析K线图，寻找K线图中“早晨之星”或“黄昏之星”形态，并参考这些形态来预测股票未来的走势情况。这种分析方法有一定程度的个人主观性，不同的人在K线图中寻找出的“早晨之星”或“黄昏之星”形态的日期可能会不同。若事先定义好形态的特征，并尝试编写代码让计算机自动捕捉此形态，则会减少一定的人为主观性。以“早晨之星”为例，先规定和量化“早晨之星”形态的要求，并尝试用Python来捕捉“早晨之星”。接下来，以上证综指 2012 年日 K 线图的分析依据，用 Python 捕捉上证综指 K 线图中出现的“早晨之星”形态。

##### (1) 确定“早晨之星”的形态

对连续 3 天的日度数据进行如下分析。

- **刻画蜡烛实体**：第 1 天（前二期）的收盘价低于开盘价，即描述蜡烛绿色实体；第 2 天（前一期）的收盘价和开盘价大致相等，两者差额控制在一个范围内；第 3 天（当期）蜡烛红色实体用收盘价高于开盘价来定义，两者的差值要大于等于第 1 天（前二期）开盘价与收盘价差值的一半。
- **定义十字星实体位置**：第 2 天（前一期）的收盘价和开盘价均须小于第 1 天（前二期）的收盘价和第 3 天（当期）的开盘价。
- **定义下跌趋势**：用收盘价来计算股票的收益率，收益率为负表示下跌。

##### (2) Python 捕捉“早晨之星”的形态

首先，获取上证综指 2012 年的日度交易数据。

```python
# 获取上证综指 2012 年日度交易数据
# 日期为 2012 年 1 月 1 日到 2012 年 12 月 31 日
In [1]: ssec2012=pd.read_csv('ssec2012.csv')
In [2]: ssec2012.index=ssec2012.iloc[:,1]
In [3]: ssec2012.index=pd.to_datetime(ssec2012.index,\
...:                 format='%Y-%m-%d')
```

```python
In [4]: ssec2012=ssec2012.iloc[:,2:]
In [5]: ssec2012.head(2)
Out[5]:
          Open       High       Low       Close  Volume
Date
2012-01-04  2211.995117  2217.520020  2168.644043  2169.389893  49200
2012-01-05  2160.896973  2183.404053  2145.555908  2148.451904  58800
```

```python
In [6]: ssec2012.iloc[-2:,:]
Out[6]:
          Open       High       Low       Close  Volume
Date
2012-12-28  2207.913086  2234.868896  2204.002930  2233.251953  116000
2012-12-31  2236.460938  2269.511963  2236.460938  2269.127930  128000
```

```python
# 提取收盘价数据
In [7]: Close=ssec2012.Close
# 提取开盘价数据
In [8]: Open=ssec2012.Open
```

接着，用 Python 捕捉蜡烛绿色实体、红色实体和十字星的实体部分，为此需要计算每一个交易日期的收盘价与开盘价的差值 ClOP。

```python
# 计算每一个交易日期的收盘价与开盘价的差值 ClOp
In [9]: ClOp=Close-Open
...: ClOp.head()
Out[9]:
Date
2012-01-04 -42.605224
2012-01-05 -12.445069
2012-01-06 15.248047
2012-01-09 61.148926
2012-01-10 63.911865
dtype: float64
```

```python
# 简要总结收盘价与开盘价差值的分布情况
In [10]: ClOp.describe()
Out[10]:
count 243.000000
mean 2.244368
std 22.884043
min -73.685059
25% -11.442016
50% 1.513183
75% 14.296997
max 91.843994
dtype: float64
```

```python
# 捕捉绿色实体、十字星和红色实体
In [11]: Shape=[0,0,0]
In [12]: lag1ClOp=ClOp.shift(1)
...... lag2ClOp=ClOp.shift(2)
In [13]: for i in range(3,len(ClOp)):
...... if all([lag2ClOp[i]<-11,abs(lag1ClOp[i])<2,\
...... ClOp[i]>6,abs(ClOp[i])>abs(lag2ClOp[i]*0.5)]):
......:
...... Shape.append(1)
......:
...... else:
......: Shape.append(0)
......:
......:
```

```python
# 查看Shape中元素第一次取值为1所在的index
In [14]: Shape.index(1)
Out[14]: 165
```

然后，定义十字星实体的位置，十字星实体要在其前后绿色实体和红色实体的下方。

```python
# 准备数据
In [15]: lagOpen=Open.shift(1)
...... lagClose=Close.shift(1)
...... lag2Close=Close.shift(2)
```

```python
# 捕捉符合十字星位置的蜡烛图
In [16]: Doji=[0,0,0]
In [17]: for i in range(3,len(Open),1):
...... if all([lagOpen[i]<Open[i],lagOpen[i]<lag2Close[i],\
...... lagClose[i]<Open[i],(lagClose[i]<lag2Close[i])]):
...:
Doji.append(1)
...:
else:
...:
Doji.append(0)
...:
...
In [18]: Doji.count(1)
Out[18]: 12
```

接下来，我们尝试刻画下跌趋势。

```python
#定义下跌趋势
#先计算收益率
In [19]: ret=Close/Close.shift(1)-1
In [20]: lag1ret=ret.shift(1)
...: lag2ret=ret.shift(2)

#寻找向下趋势
In [21]: Trend=[0,0,0]
...: for i in range(3,len(ret)):
...:	if all([lag1ret[i]<0,lag2ret[i]<0]):
...:		Trend.append(1)
...:	else:
...:		Trend.append(0)
```

上述3个条件刻画完以后，用Python自动寻找“早晨之星”形态。

```python
In [22]: StarSig=[]
...: for i in range(len(Trend)):
...:	if all([Shape[i]==1,Doji[i]==1,Trend[i]==1]):
...:		StarSig.append(1)
...:	else:
...:		StarSig.append(0)

#捕捉上证指数2012年出现“早晨之星”形态的日期
In [23]: for i in range(len(StarSig)):
...:	if StarSig[i]==1:
...:		print(ssec2012.index[i])
...
...
2012-09-06 00:00:00
```

对上证指数 2012 年的交易信息的“早晨之星”形态进行捕捉，从代码结果中可以看出，2012 年上证指数的 K 线图上只出现了 1 次“早晨之星”形态，日期为 2012 年 9 月 6 日，我们绘制出 2012 年 9 月 6 日附近的 K 线图。如图 27.5 所示。

```python
#提取交易数据
In [24]: ssec201209=ssec2012['2012-08-21':'2012-09-30']

#绘制K线图
In [25]: import candle
In [26]: candle.candlePlot(ssec201209,\
...:					title='2012年9月6日附近K线图')
```

![](img/0de14a685304f8aa267f6492145864ac_428_0.png)

图 27.5 上证综指 2012 年 9 月份的日 K 线图

观察图 27.5，股价先处于短期下跌趋势，又经过了短期的盘整，接着在 9 月 6 日附近出现了“早晨之星”形态，该形态释放出市场反转的信号。然后，9 月 6 日以后的几个交易日，股价处于上涨趋势。

### 27.3.2 Python 语言捕捉“乌云盖顶”形态

#### 1. “乌云盖顶”与“抱线”

“乌云盖顶”形态一般由两个不同矩形实体的蜡烛图组成。第 1 个蜡烛图属于上升趋势的阳线，当日收盘时的价格比开盘时的价格高。红色蜡烛实体较长表明收盘价比开盘价高很多；第 2 个蜡烛图也有一个较长的实体，且实体上端（开盘价）一般要高于前一个蜡烛图的实体上端（收盘价）。第二天开市时价格较高，说明多头市场可能依旧强势，如果开盘价比前一天的最高价还高，说明开市时多头力量更大。而闭市时，价格却大幅下跌，收盘价低于开盘价。价格下跌的一个可能情景是市场上出现了不利消息，股价受到不利影响，多头力量削弱，空头力量强势起来。一般来说，第二天的收盘价要深入到第一天蜡烛图红色实体的内部，第二天的收盘价向下穿入第一个红色实体的内部越低，市场下跌趋势的信号就越明显。

“抱线”有很多种形态，如图 27.6 所示的抱线仅是抱线众多形态中的一种，是“阴抱阳”形态。这种形态的抱线也常被叫作“看跌吞没”形态。“看跌吞没”形态一般要求第二个绿色蜡烛实体包裹着第一个红色蜡烛实体。从价格角度来看，第二天的收盘价要低于前一天的开盘价，第二天的开盘价高于前一天的收盘价。

![](img/0de14a685304f8aa267f6492145864ac_429_0.png)

图 27.6 乌云盖顶与抱线

#### 2. Python 捕捉上证综指 “乌云盖顶” 形态

以 “乌云盖顶” 形态为例，我们尝试运用 Python 捕捉上证综指 2011 年的 K 线图中的 “乌云盖顶” 形态。

##### （1） 定义 “乌云盖顶” 形态

- 第 1 个（前一期）红色蜡烛实体：收盘价高于开盘价；第 2 个（当期）绿色蜡烛实体：收盘价低于开盘价；
- 第 2 天（当期）的开盘价高于第 1 天（前一期）的收盘价；第 2 天（当期）收盘价位于第 1 天（前一期）实体的下半部分范围内，即第 2 天（当期）收盘价低于第 1 天（前一期） 收盘价与开盘价之和的一半，且大于第 1 天（前一期）的开盘价；
- 定义前期上升趋势：连续两期的收益率为正。

##### （2） Python 捕捉 “乌云盖顶” 形态

使用上证综指 2011 年的日交易数据，编写 Python 代码捕捉 “乌云盖顶” 形态。

```python
#提取读入上证综指2011年的日交易数据
In [1]: import pandas as pd
In [2]: ssec2011=pd.read_csv('ssec2011.csv')
     ssec2011.index=ssec2011.iloc[:,1]
     ssec2011.index=pd.to_datetime(ssec2011.index,\
     format='%Y-%m-%d')
     ssec2011=ssec2011.iloc[:,2:] 

#提取价格数据
In [3]: Close11=ssec2011.Close
     Open11=ssec2011.Open

#判断捕捉符合 “乌云盖顶” 形态的连续两个蜡烛实体
In [4]: lagClose11=Close11.shift(1)
     lagOpen11=Open11.shift(1)
     Cloud=pd.Series(0,index=Close11.index)
     for i in range(1,len(Close11)):
     if all([Close11[i]<Open11[i],\
     lagClose11[i]>lagOpen11[i],\
     Open11[i]>lagClose11[i],\
     Close11[i]<0.5*(lagClose11[i]+lagOpen11[i]),\
     Close11[i]>lagOpen11[i]]):
     Cloud[i]=1
```
