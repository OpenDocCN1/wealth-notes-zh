
(a) 绘制中国交通银行股票的K线图；

(b) 计算6日RSI、30日RSI的值，并绘制出两者的曲线图；

(c) 设定6日RSI取值大于90，为超买区，释放出卖出信号；6日RSI取值小于10时，为超卖区，释放出买入信号。编写Python代码计算这种买卖信号预测的准确率；

(d) 当6日RSI线向上穿过30日RSI线时，为“黄金交叉”，释放出买入信号；当6日RSI线向下穿过30日RSI线时，为“死亡交叉”，释放出卖出信号。根据长短线的“黄金交叉”与“死亡交叉”策略，编写Python代码实现这一策略，并进行回测；

3. “RSI将叉不叉买入策略”：当短期RSI第一次向上穿过长期RSI时，为“黄金交叉”，此后，短期RSI可能要向下突破长期RSI，在短期RSI即将向下突破长期RSI时，没有向下突破长期RSI而是反转向上走，短期RSI线再次在长期RSI线的上方，此时出现了“将叉不叉”现象，此处表明市场向上走势较强，释放出强烈的买入信号；

以第2题中国交通银行6日RSI和30日RSI为短线和长线，用Python捕捉上文所描述的现象，并计算其买入信号预测的准确率。

## 第30章 均线系统策略

做投资的朋友大概都不会反对“趋势是我们的朋友”这句话。趋势交易策略长期以来被认为具有良好的获利能力，因此，交易员常围绕它思考两个问题：如何判断趋势和如何利用它进行交易。捕捉趋势最普遍的方法为移动平均线，根据求平均的方式不同，可以有简单移动平均数（Simple Moving Average，SMA）、指数加权移动平均数（Weighted Moving Average，WMA）和指数移动平均数（Exponential Moving Average，EXPMA 或 EMA）。

### 30.1 简单移动平均

#### 30.1.1 简单移动平均数

回顾一下求解算术平均数的方法，即先对一组数据的值求和，再用这个总和除以这组数据的个数来得到算术平均数。同理，股价的简单移动平均数就是将一组股价值相加，再除以股价的个数。因此，需要先确定对哪几个数求平均数，即确定股价的个数 $n$。一般说来，我们以日、周为单位，比如求 5 日的平均数，10 日的平均数，20 日的平均数或者 3 周的平。此外，还请注意，为了体现“移动”两个字的作用，在求得第一个平均数以后，若要求第二个平均数，就要把原来的一组数的最早一项股价减去，再加上新一项新的股价，求和后除以股价的个数，以得到第二个平均数。

根据定义，股票在第 1 天到第 4 天无法求出 5 日价格简单移动平均数。第 5 天的简单平均数为：

$$\mathrm{SMA}_{t=5}=\frac{p_{1}+p_{2}+p_{3}+p_{4}+p_{5}}{5}$$

式中， $p_{1}$ 、 $p_{2}$ 、 $p_{3}$ 、 $p_{4}$ 、 $p_{5}$ 分别表示股票第 1 天、第 2 天、第 3 天、第 4 天、第 5 天的价格。

第 6 天的 5 日简单移动平均数则为：

$$\mathrm{SMA}_{t=6}=\frac{p_{2}+p_{3}+p_{4}+p_{5}+p_{6}}{5}$$

即去掉第一天的价格，加上第 6 天的价格 $p_{6}$ ，然后除以 5。同理，计算出第 7 天及之后的简单移动平均数，再用这些数据绘制时间序列图，就可以得到股价的简单移动平均曲线。

首先，读取青岛啤酒 2014 年以来的交易数据，提取收盘价，并绘制收盘价时序图。如图 30.1 所示。

```python
# 获取青岛啤酒交易数据
In [1]: import pandas as pd
        import numpy as np
        import matplotlib.pyplot as plt

In [2]: TsinTao=pd.read_csv('TsinTao.csv')
...: TsinTao.index=TsinTao.iloc[:,1]
...: TsinTao.index=pd.to_datetime(TsinTao.index, format='%Y-%m-%d')
...: TsinTao=TsinTao.iloc[:,2:]
...: TsinTao.head(n=3)

Out [2]:
      Date    Open   High    Low  Close  Volume
0  2014-01-02  48.80  48.98  46.90  47.81  2592800
1  2014-01-03  47.60  48.38  47.01  47.59  1560700
2  2014-01-06  47.63  47.79  46.62  46.70  1860900

#提取收盘价数据
In [3]: Close=TsinTao.Close

#绘制收盘价数据时序图
In [4]: plt.rcParams['font.sans-serif'] = ['SimHei']
...: plt.subplot(111)
...: plt.plot(Close,'k')
...: plt.xlabel('date')
...: plt.ylabel('Close')
...: plt.title('2014年青岛啤酒股价走势时序图')
Out[4]: <matplotlib.text.Text at 0x1d52bc53b70>
```

![](img/0de14a685304f8aa267f6492145864ac_465_0.png)

**图 30.1 2014 年青岛啤酒股票收盘价时序图**

接下来，用 Python 编写代码演示青岛啤酒收盘价的简单移动平均数的求值过程。如图 30.2 所示。

```python
In [5]: SMA5=pd.Series(0.0,index=Close.index)

In [6]: for i in range(4,len(Close)):
...:     SMA5[i]=sum(Close[(i-4):(i+1)])/5

In [7]: SMA5.tail()
Out[7]:
    Date
    2015-04-24  46.522
    2015-04-27  46.462
    2015-04-28  45.936
    2015-04-29  45.430
    2015-04-30  44.950
    dtype: float64

In [8]: plt.plot(Close[4:],label="Close",color='g')
...: plt.plot(SMA5[4:],label='SMA5',color='r',linestyle='dashed')
...: plt.title("青岛啤酒收盘价及其简单移动平均线图")
...: plt.ylim(35,50)
...: plt.legend()
Out[8]: <matplotlib.legend.Legend at 0xbf9aa20>
```

![](img/0de14a685304f8aa267f6492145864ac_466_0.png)

**图 30.2 青岛啤酒 5 日简单移动平均线**

#### 30.1.2 简单移动平均函数

用 Python 编写一个计算k 期简单移动平均函数 smaCal()，以实现数据的简单移动平均。具体代码如下：

```python
In [1]: def smaCal(tsPrice,k):
...:     import pandas as pd
...:     Sma=pd.Series(0.0,index=tsPrice.index)
...:     for i in range(k-1,len(tsPrice)):
...:         Sma[i]=sum(tsPrice[(i-k+1):(i+1)])/k
...:     return(Sma)
```

运用 smaCal( ) 函数计算简单移动平均价格。

```python
In [2]: sma5=smaCal(Close,5)
In [3]: sma5.tail()
Out[3]:
Date
2015-04-24    46.522
2015-04-27    46.462
2015-04-28    45.936
2015-04-29    45.430
2015-04-30    44.950

dtype: float64
```

#### 30.1.3 期数选择

移动平均的期数的选择对简单移动平均的修匀效果影响很大，要确定移动平均的期数，一般需要从以下三个方面考虑。

1. 事件发展的周期性。如果事件的发展具有周期性，一般应以周期长度作为移动平均的间隔长度。比如研究每年的平均气温变化趋势，对其做 12 期移动平均，以便通过周期平均消除季节效应的影响。
2. 对趋势平均性的要求。一般来说，移动平均的期数越多，修匀效果越平均，表现出的趋势就越清晰。
3. 对趋势反映近期变化敏感程度的要求。用移动平均方法确定事件的发展趋势都具有一定的滞后性。移动平均的期数越多，滞后性越大；移动平均的期数越少，所得的趋势图对近期变化的反应就越敏感。因此，如果想得到长期趋势，最好做期数比较大的移动平均；如果想密切关注序列的短期趋势，最好做期数比较小的移动平均。

### 30.2 加权移动平均

#### 30.2.1 加权移动平均数

加权移动平均数与简单移动平均数不同，其并不是简单地把股票价格相加，而是对股价赋予一定的权重再相加。为什么会有加权移动平均线呢？回想一下，对股票价格求平均的目的即是得到一条能够代表股票价格的相对平均的曲线。根据人们对股票市场的一般认知，昨天的数据会比 10 天前的价格更能反映今天的股价情况。离当前时间越近的数据越具有代表性，越久远的数据越没有代表性。因此，在预测股价时，不同时期的股价数据具有不同的代表性。为了表示其代表性的高低，可以考虑先对股价赋予一定的权重，再求平均值。至于数据的选择，加权移动平均与简单移动平均采用了相同的方法。还是以 5 日的时间长度为例，5 日加权移动平均值的计算公式为：

$$WMA_{t=5}=w_{1}p_{1}+w_{2}p_{2}+w_{3}p_{3}+w_{4}p_{4}+w_{5}p_{5}$$

式中，$w_{1}$、$w_{2}$、$w_{3}$、$w_{4}$、$w_{5}$ 为股价数据的权重且 $w_{1}+w_{2}+w_{3}+w_{4}+w_{5}=1$。同理，得出第 6 天及以后的加权移动平均数，再用这些数据绘制时间序列图，即可得到股价的加权移动平均曲线。

继续以青岛啤酒股票数据为例，求 5 日加权移动平均值。前面已经获取青岛啤酒的收盘价格数据，接下来定义权重，进而求加权移动平均值。如图 30.3 所示。

```python
#定义权重
In [1]: b=np.array([1,2,3,4,5])
     .: w=b/sum(b)
Out[1]: array([0.06666667, 0.13333333, 0.2 , 0.26666667, 0.33333333])

#根据青岛啤酒前5日收盘价来演示第5个交易日的加权平均数的求法
In [2]: n1Close=Close[0:5]
     .: wec=w*n1Close
     .: sum(wec)
Out[2]: 46.778666666666666

In [3]: Wma5=pd.Series(0.0,index=Close.index)
     .: for i in range(4,len(Close)):
     .:     Wma5[i]=sum(w*Close[i-4:(i+1)])
     .: 
     .: Wma5[2:7]
Out[3]:
Date
2014-01-06    0.000000
2014-01-07    0.000000
2014-01-08    46.778667
2014-01-09    46.250667
2014-01-10    45.710667
dtype: float64

In [4]: plt.rcParams['font.sans-serif'] = ['SimHei']
     .: plt.plot(Close[:], label="Close", color='g')
     .: plt.plot(Wma5[:], label="5wma", color='-', linestyle='dashed')
     .: plt.title("自适应调仓策略, 简单移动")
     .: plt.ylim(35,50)
     .: plt.legend()
Out[4]: <matplotlib.legend.Legend at 0x1d52be38ba8>
```

![](img/0de14a685304f8aa267f6492145864ac_469_0.png)

**图 30.3 青岛啤酒 5 日加权移动平均线**

#### 30.2.2 加权移动平均函数

用 Python 编写一个加权移动平均的函数 wmaCal( )，以实现数据的加权移动平均。wmaCal( ) 函数定义代码如下：

```python
In [1]: def wmaCal(tsPrice,weight):
         import pandas as pd
         import numpy as np
         k=len(weight)
         arrWeight=np.array(weight)
         Wma=pd.Series(0.0,index=tsPrice.index)
         for i in range(k-1,len(tsPrice.index)):
             Wma[i]=sum(arrWeight*tsPrice[(i-k+1):i+1])
         return(Wma)
```

用青岛啤酒股票的日收盘价数据来说明一下 wmaCal( ) 函数的使用方法。

```python
In [2]: wma5=wmaCal(Close,w)
         wma5.head()
Out[2]:
Date
2014-01-02    0.000000
2014-01-03    0.000000
2014-01-06    0.000000
2014-01-07    0.000000
2014-01-08    46.778667
dtype: float64

In [3]: wma5_weight=wmaCal(Close,[0.1,0.15,0.2,0.25,0.3])
...: wma5_weight.tail()
Out[3]:
Date
2015-04-24    46.4585
2015-04-27    46.3580
2015-04-28    45.6385
2015-04-29    45.1405
2015-04-30    44.6605
dtype: float64
```

### 30.3 指数加权移动平均

#### 30.3.1 指数加权移动平均数

实际上，指数加权移动平均数相当于一种特别的加权移动平均。在此仍以 5 日指数移动平均为例。我们先给定一个权重值，比如 0.2。由加权移动平均的定义可知，我们无法得到前 4 期的 5 日加权移动平均数，而只能得到从 5 期开始的平均数。假设第 5 期的 5 日加权移动平均数为 56.00，第 6 天的股票价格为 67.00，第 7 天的股价为 60.00。如果我们在计算第 6 期的加权移动平均数时，将第 5 期的加权移动平均数与第 6 期的股价作为加权的变数，将 0.8 和 0.2 作为二者的权重值，则第 6 期的加权移动平均数为 $0.8 \times 56.00 + 0.2 \times 67.00 = 58.20$。运用同样的方法，第 7 天的加权平均数为 $0.8 \times 58.20 + 0.2 \times 60.00 = 58.56$，如表 30.1 所示。

表 30.1 EWMA 计算表

| 日期 | 股价 | 权重 | EWMA |
| :--- | :--- | :--- | :--- |
| 5 |  | 0.2 | 56.00 |
| 6 | 67.00 | 0.2 | $0.8 \times 56.00 + 0.2 \times 67.00 = 58.20$ |
| 7 | 60.00 | 0.2 | $0.8 \times 58.20 + 0.2 \times 60.00 = 58.56$ |

将上述计算方法一般化，假设 $p_t$ 表示股票第 $t$ 期的价格，我们从第 $k$ 期开始计算股价的加权移动平均数，且第 $k$ 期的平均数计算非常简单，比如用前 $k$ 期的股价之简单平均数求得，即：

$$\mathrm{EWMA}_{t=k} = \frac{p_1 + p_2 + \cdots + p_k}{k}$$

从第 $k+1$ 期开始，每一期的移动平均数为当期股价与上一期移动平均数之加权平均，权重分别为 $\alpha$ 和 $1-\alpha$，即：

$$\mathrm{EWMA}_{t=k+1} = p_{k+1} \times \alpha + \mathrm{EWMA}_{t=k} \times (1-\alpha)$$

第 $k+2$ 期指数加权移动平均数为：

$$\mathrm{EWMA}_{t=k+2} = p_{k+2} \times \alpha + \mathrm{EWMA}_{t=k+1} \times (1-\alpha)$$

$$= p_{k+2} \times \alpha + \left[ p_{k+1} \times \alpha + \mathrm{EWMA}_{t=k} \times (1-\alpha) \right] \times (1-\alpha)$$

$$= p_{k+2} \times \alpha + p_{k+1} \times \alpha \, (1-\alpha) + \mathrm{EWMA}_{t=k} \times (1-\alpha)^2$$

依此类推，我们可以得出之后各期加权移动平均数。总的来说，当 $t \ge k + 1$ 时，有：

$$\mathrm{EWMA}_{t} = \alpha \left[ p_{t} + p_{t-1} \times (1-\alpha) + \cdots + p_{k+1}(1-\alpha)^{t-k-1} \right] + \mathrm{EWMA}_{t=k} \times (1-\alpha)^{t-k}$$

式中，$\mathrm{EWMA}_{t=k} = \frac{p_1+p_2+\cdots+p_k}{k}$ 。在公式中，我们可以看到加权移动平均数是权重 $(1-\alpha)$ 的指数函数，因此这种计算移动平均数的方法被称为指数加权移动平均。而且根据公式还可以看出，股价的指数加权移动平均数 $\mathrm{EWMA}_t$ 是过去所有期的价格 $(p_1, p_2, \cdots, p_t)$ 之加权平均，从当期往过去价格之权重呈现指数递减现象。若将各期指数加权移动平均数用折线图绘制出来，则可以得到股价的指数移动平均曲线。

还是以青岛啤酒股票为例，计算该股票收盘价的指数移动平均。再把指数移动平均用折线图绘制出来，就可以得到股价的指数移动平均曲线。如图 30.4 所示。

```python
In [1]: Ema5_number1=np.mean(Close[0:5])

#计算第 6 天的指数移动平均数
In [2]: Ema5_number2=0.2* Close[5]+(1-0.2)*Ema5_number1
In [3]: Ema5=pd.Series(0.0,index=Close.index)
....:  Ema5[4]=Ema5_number1
....:  Ema5[5]=Ema5_number2

#计算第 7 天及以后的指数移动平均数
In [4]: for i in range(6,len(Close)):
....:      expo=np.array(sorted(range(i-4),reverse=True))
....:      w=(1-0.2)**expo
....:      Ema5[i]=sum(0.2*w*Close[5:(i+1)])+Ema5_number1*0.2**i-5
....:  
In [5]: Ema5.tail()
Out[5]: 
Date
2015-04-24    46.283839
2015-04-27    46.263071
2015-04-28    45.832457
2015-04-29    45.555965
2015-04-30    45.242772
dtype: float64

In [6]: plt.rcParams['font.sans-serif'] = ['SimHei']
....: plt.plot(Close[4:],label="Close",color='k')
....: plt.plot(Ema5[4:],label="Ema5",\
....:      color='g',linestyle='--')
....: plt.title("各周期移动平均线与股价走势对照")
....: plt.ylim(35,50)
....: plt.legend()
Out[6]: <matplotlib.legend.Legend at 0x1d52bed9908>
```
