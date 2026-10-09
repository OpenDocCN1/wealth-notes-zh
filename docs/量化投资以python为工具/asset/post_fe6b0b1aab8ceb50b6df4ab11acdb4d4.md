
## 第 29 章 RSI 相对强弱指标

### 29.6 RSI 的“黄金交叉”与“死亡交叉”

![](img/0de14a685304f8aa267f6492145864ac_455_0.png)

图 29.2 RSI6 的超买线和超卖线

超买线和超卖线的制定可以有所变化，RSI 值为 90 和 10 也可以作为股票超买线和超卖线的制定标准。此外，RSI 值为 70 和 30 的超卖线和超买线已被验证能有效预测未来股价的走势，并为投资者和学者广泛使用。

在股票市场使用 RSI 指标时，一般会定义不同时间跨度的 RSI 值。当短期 RSI 线向上穿长期 RSI 线时，股票近期买入的力量较强，价格上涨的力量很大，其释放出一个较强的买入信号，这个信号被称为“黄金交叉”。当短期 RSI 线向下跌破长期 RSI 线时，股票近期卖出的力量较强，价格下跌的力量很大，其释放出一个较强的卖出信号，被称为“死亡交叉”。

比如，以交通银行股价为例，用 Python 绘制出 RSI 的黄金交叉和死亡交叉线。

```python
In [12]: plt.plot(Rsi6['2015-01-03':], label='Rsi6')
....: plt.plot(Rsi24['2015-01-03':],
....:             label='Rsi24', color='red', \
....:             linestyle='dashed')
....: plt.title("RSI ... .")
....: plt.ylim(-10,110)
....: plt.legend()
Out[12]: <matplotlib.legend.Legend at 0x29c4c9f8b00>
```

如图 29.3 所示，我们定义 6 日 RSI 为短期 RSI，24 日 RSI 为长期 RSI。观察 RSI6（实线）和 RSI24（虚线）走势图，可以看出 RSI6 曲线在 0 ～ 100 之间上下波动，且波动相对较大。RSI24 在 20 ～ 80 之间取值，曲线较为平滑。从超买和超卖的角度分析，当 RSI6 取值超过 80 时，股价处于超买区，预期股票价格将要下跌。对比交通银行收盘价时序图与 RSI6 曲线图，当 RSI6 取值在 80 以上时，收盘价大致处于下降阶段。当 RSI6 取值低于 20 时，股票处于超卖区，卖出的力量过大，股票价格可能要回升。从 RSI 的长短线来分析，从图 29.3 上可以看出，RSI6 曲线（实线）在很多情况下都向上穿过 RSI24 曲线（虚线），出现不少“黄金交叉”信号。对照收盘价时序图，可以看出“黄金交叉”信号附近股价大致处于上涨行情。图 29.3 也有不少“死亡交叉”信号出现，RSI6 曲线（实线）从上向下穿过 RSI24 曲线（虚线）。

![](img/0de14a685304f8aa267f6492145864ac_456_0.png)

图 29.3 RSI 的黄金交叉和死亡交叉

### 29.7 交通银行股票 RSI 指标交易实测

以交通银行股票为例，用 Python 编写代码计算出 RSI 值，并运用 RSI 值制定交易策略。编写代码的主要思路如下。

- 1. 数据准备：获取交通银行股票数据，提取出收盘价数据。
- 2. 捕捉买卖点：计算 RSI6 和 RSI24 的值，当 RSI6 > 80 或者 RSI6 向下穿过 RSI24 时，为卖出信号。当 RSI6 < 20 或者 RSI6 向上穿过 RSI24 时，为买入信号。
- 3. 交易策略执行：按照 RSI 的买卖点买入或者卖出股票。
- 4. 回测：对交易策略进行投资评价。

#### 29.7.1 RSI 捕捉交通银行股票买卖点

（1）在如下代码中，股票交易数据是计算 RSI 指标并且制定交易策略的基础。在数据准备方面，本章的前面内容已经提取出收盘价数据。我们再次重新梳理一遍读取交通银行股票交易数据以及收盘价的代码。

```python
# 提取交通银行股票的交易数据
In [1]: BOCM=pd.read_csv('BUCH.CSV')
      ....: BOCM.index=BOCM.iloc[:,1]
      ....: BOCM.index=pd.to_datetime(BOCM.index, format='%Y-%m-%d')
      ....: BOCMClp=BOCM.Close
      ....: BOCMClp[0:4]
Out[1]:
Date
2014-01-02    3.82
2014-01-03    3.79
2014-01-06    3.75
2014-01-07    3.77
Name: Close, dtype: float64
```

（2）计算交通银行 RSI 的值并捕捉买卖点信号。

```python
In [2]: rsi6=rsi(BOCMClp,6)
      ....: rsi24=rsi(BOCMClp,24)
```

```python
#rsi6 的超买与超卖
In [3]: Sig1=[]
      ....: for i in rsi6:
      ....: if i>80:
      ....: Sig1.append(-1)
      ....: elif i<20:
      ....: Sig1.append(1)
      ....: else:
      ....: Sig1.append(0)
```

```python
In [4]: date1=rsi6.index
      ....: Signal1=pd.Series(Sig1,index=date1)
```

```python
In [5]: Signal1[Signal1==1].head(n=3)
Out[5]:
Date
2014-01-20    1
2014-01-21    1
2014-02-06    1
dtype: int64
```

```python
In [6]: Signal1[Signal1==-1].head(n=3)
Out[6]:
Date
2014-02-11   -1
2014-02-12   -1
2014-02-13   -1
dtype: int64
```

```python
#交易信号二：黄金交叉与死亡交叉
In [7]: Signal2=pd.Series(0,index=rsi24.index)
      ....: lagrsi6= rsi6.shift(1)
      ....: lagrsi24= rsi24.shift(1)
```

```python
...: for i in rsi24.index:
...: if (rsi6[i]>rsi24[i]) & (lagrsi6[i]<lagrsi24[i]):
...: Signal2[i]=1
...: elif (rsi6[i]<rsi24[i]) & (lagrsi6[i]>lagrsi24[i]):
...: Signal2[i]=-1
...
```

Signal1 的取值 1 表明 rsi6 低于超卖线，预测未来价格要回升，释放出买入信号；Signal2 的取值 1 时，表明短期 rsi6 从下向上穿过长期 rsi24，价格可能有上升的趋势，释放出买入的信号；Signal1 取值 -1 时，表示 rsi6 的值高出超买线，价格可能有回落的趋势，释放出卖出信号；Signal2 取值 -1 时，表示表明短期 rsi6 从上向下穿过长期 rsi24，释放出卖出的信号。

（3）将这两种交易信号结合起来，进而确定中国交通银行股票的最终买卖信号。

```python
# 合并交易信号
In [8]: signal=Signal1+Signal2
...: signal[signal>1]=1
...: signal[signal<=-1]=-1
...: signal=signal.dropna()
```

#### 29.7.2 RSI 交易策略执行及回测

运用 RSI 指标捕捉交通银行的买卖点，RSI 指标无法对每个交易日数据进行买入或者卖出的判断。在大部分交易日中，RSI 取值在正常范围内，只有个别交易日期 RSI 突破超买线或者超卖线，这些交易日期有可能间隔时间较长。接下来，根据 RSI 释放的买入和卖出信号执行 RSI 交易策略，并评价交易策略。

```python
In [9]: tradSig=signal.shift(1)

#求交通银行股票收益率
In [10]: ret=BDCMClp/BDCMClp.shift(1)-1
...: ret.head()
Out[10]:
Date
2014-01-02 NaN
2014-01-03 -0.007853
2014-01-06 -0.010554
2014-01-07 0.005333
2014-01-08 0.007958
Name: Close, dtype: float64
```

```python
#求买入信号收益率
In [11]: ret=ret[tradSig.index]
...: buy=tradSig[tradSig==1]
...: buyRet=ret[tradSig==1]*buy

#求卖出信号收益率
In [12]: sell=tradSig[tradSig==-1]
...: sellRet=ret[tradSig==-1]*sell
```

```python
#求出买卖交易合并的收益率
In [13]: tradeRet=ret*tradSig
```

- 绘制三种交易收益率的时序图，展现收益率的分布情况，如图 29.4 所示。

```python
In [14]: plt.rcParams['font.sans-serif'] = ['SimHei']
..... plt.rcParams['axes.unicode_minus'] = False
..... plt.subplot(211)
..... plt.plot(buyRet,label='buyRet',color='g')
..... plt.plot(sellRet,label="sellRet",color='r',linestyle='dashed')
..... plt.title('RSI 指标交易策略')
..... plt.ylabel('strategy return')
..... plt.legend()
..... plt.subplot(212)
..... plt.plot(ret,'b')
..... plt.ylabel('stock return')
Out[14]: <matplotlib.text.Text at 0x29c4cacccbe0>
```

![](img/0de14a685304f8aa267f6492145864ac_459_0.png)

图 29.4 RSI 策略收益率时序图

- 计算信号点预测准确率情况，求预测正确时的平均收益率与预测失败时的平均收益率。

```python
In [15]: def strat(tradeSignal,ret):
....:     indexDate=tradeSignal.index
....:     ret=ret[indexDate]
....:     tradeRet=ret*tradeSignal
....:     tradeRet[tradeRet==-(-0)]=0
....:     vinRate=len(tradeRet[tradeRet>0])/len(
....:       tradeRet[tradeRet!=0])
....:     meanWin=sum(tradeRet[tradeRet>0])/len(
....:       tradeRet[tradeRet>0])
....:     meanLoss=sum(tradeRet[tradeRet<0])/len(
....:       tradeRet[tradeRet<0])
....:     perform={'vinRate': vinRate,\
....:         'meanWin': meanWin,\
....:         'meanLoss': meanLoss}
....:     return(perform)
```

```python
#计算买入点、卖出点和整个交易点的
#预测获胜率、平均获胜收益率与平均损失收益率
In [16]: BuyOnly=strat(buy,ret)
.....     SellOnly=strat(sell,ret)
.....     Trade=strat(tradSig,ret)
.....     Test=pd.DataFrame({'BuyOnly':BuyOnly,\
.....                         'SellOnly':SellOnly,'Trade':Trade})
.....     Test
Out[16]:
          BuyOnly  SellOnly     Trade
meanLoss -0.009230 -0.028476 -0.019797
meanWin   0.012996 0.015883 0.014691
winRate   0.530612 0.569231 0.547826
```

- 比较 RSI 指标交易策略的累计收益率。

```python
In [17]: cumStock=np.cumprod(1+ret)-1
.....     cumTrade=np.cumprod(1+tradeRet)-1
```

```python
In [18]: plt.subplot(211)
.....     plt.plot(cumStock)
.....     plt.ylabel('cumStock')
.....     plt.title('股票本身累计收益率')
.....     plt.subplot(212)
.....     plt.plot(cumTrade)
.....     plt.ylabel('cumTrade')
.....     plt.title('rsi策略累计收益率')
```

如图 29.5 所示，该绩效不是很理想，我们可以考虑调整制定交易策略时所用的各种参数来改进 RSI 交易策略之绩效表现。在前文中，RSI 指标释放出的买卖点信号 1 天后执行买卖操作；然而，当 RSI 指标释放出买卖点信号以后，我们观察到价格没有及时上涨或者下跌。下面据此修正买卖信号执行点，即 RSI 指标释放出的买卖点信号，再隔 3 天以后进行执行买卖操作，然后进行交易回溯。

![](img/0de14a685304f8aa267f6492145864ac_461_0.png)

图 29.5 RSI 指标交易策略表现

```python
#计算修正后交易策略的收益率表现
In [19]: tradSig2=sigmal.shift(3)
            ...: ret2=ret[tradSig2.index]
            ...: buy2=tradSig[tradSig2==1]
            ...: sell2=tradSig[tradSig2==[-1]]*sell2
            ...: tradeRet2=ret2*tradSig2
            ...: BuyOnly2=strat(buy2,ret2)
            ...: SellOnly2=strat(sell2,ret2)
            ...: Trade2=strat(tradSig2,ret2)
            ...: Test2=pd.DataFrame({"BuyOnly":BuyOnly2, 
            ...:        "SellOnly":SellOnly2,"Trade":Trade2})
            ...: Test2

Out[19]:
            BuyOnly    SellOnly      Trade
meanLoss -0.013085 -0.016942 -0.015167
meanWin    0.018347    0.017260    0.016922
winRate    0.523810    0.537313    0.537815

In [20]: cumStock2=np.cumprod(1+ret2)-1
            ...: print(cumStock2[-1])

0.809523809524

In [21]: cumTrade2=np.cumprod(1+tradeRet2)-1
            ...: print(cumTrade2[-1])

0.299171578139
```

对比图 29.6 和图 29.5 中的累计收益率（Cumulative Return）可以看出，修正的 RSI 指标交易策略的累计收益率也比原 RSI 指标交易策略的收益率要大很多。

本章介绍的 RSI 指标交易策略相对简捷，着重运用 Python 来阐述 RSI 指标的概念与交易思想。在实际分析运用中，读者可以从以下 4 个角度来综合运用 RSI 指标。

- 1. 根据 RSI 的取值大小判断市场的超买和超卖热度；
- 2. RSI 线的形态分析也是一个考量因素，RSI 曲线在高位区或者低位区出现不同的形态，会释放出不同的买卖信号。
- 3. 长短线 RSI 的黄金交叉与死亡交叉。
- 4. RSI 线的走势与价格线走势的背离也是一大考虑因素。

此外，在运用 RSI 指标分析市场行情时，还需要考虑 RSI 指标有可能会“钝化”，比如，当市场处于疯狂牛市时，RSI 突破超买线释放出的卖出信号则没有太大意义。

![](img/0de14a685304f8aa267f6492145864ac_462_0.png)

图 29.6 修正的 RSI 指标交易策略

## 习题

- 1. （从 problem29-1.csv 获取数据）选取中国交通银行股票（股票代码为“601328”）2013 年 1 月 1 日到 2013 年 12 月 31 日的日度交易数据，编写 Python 代码分别计算 5 日、6 日、7 日、8 日、9 日、10 日、12 日 RSI 的值，并将这些值合并到一个数据框中。
- 2. （从 problem29-2.csv 获取数据）选取中国交通银行股票（股票代码为“601328”）2013 年 1 月 1 日到 2014 年 12 月 31 日的日度交易数据。
