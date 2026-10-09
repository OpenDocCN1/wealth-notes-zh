```
#布林带上下轨道突破策略
In [1]: BBands=bbands(Close,20,2)
In [2]: upBreakBB1=upbreak(Close,BBands.upBand)
     downBreakBB1=downbreak(Close,BBands.downBand)
#信号出现2天后进行交易
In [3]: upBBSig1=-upbreakBB1.shift(2)
     downBBSig1=downbreakBB1.shift(2)
In [4]: tradSignal1=upBBSig1+downBBSig1
     tradSignal1[tradSignal1==-0]=0
```

> 进行交易评价之前，先构造交易评价函数，设定函数参数为价格和交易信号。

```
In [5]: def perform(tsPrice,tsTradSig):
        ret=tsPrice/tsPrice.shift(1)-1
        tradRet=(ret*tsTradSig).dropna()
        ret=ret[-len(tradRet):]
        winRate=[len(ret[ret>0])/len(ret[ret!=0]),\
                 len(tradRet[tradRet>0])/len(tradRet[tradRet!=0])]
        meanWin=[np.mean(ret[ret>0]),\
                 np.mean(tradRet[tradRet>0])]
        meanLoss=[np.mean(ret[ret<0]),\
                  np.mean(tradRet[tradRet<0])]
        Performance=pd.DataFrame({'x:Rate':winRate,'meanWin':meanWin,\
                                 'meanLoss':meanLoss})
        Performance.index=['Stock','Trade']
        return(Performance)
```

接下来运用绩效评价函数来计算布林带交易的绩效表现和股票本身的绩效表现。下列代码结果的第一行（row）表示布林带交易策略的绩效表现，第二行（row）表示股票本身的绩效表现。

```
#计算平均损失收益率、平均获益收益率以及交易获胜率
In [6]: Performance=perform(Close,tradSignal1)
Out[6]:
        meanLoss   meanWin   winRate
Stock  -0.012599  0.013320  0.458525
Trade  -0.011710  0.014709  0.557692
```

# 31.5.2 特殊布林带通道突破策略

布林带通道的上下轨道线的刻画，由选取的时间周期和标准差倍数两部分决定。不同的时间周期参数和标准差倍数可以刻画出不同的上下轨道线，进而股价突破布林带上下轨道线的情形则会不同。如果布林带通道线设定不合理，股票价格向上穿过布林带上通道线以后，中短期内一直处于上升趋势，而不会回落到通道线内部，或者股价跌破布林带下通道线以后，中短期一直处于下降趋势，而不会反弹回通道线内部，此时，我们称该通道策略为特殊布林带通道策略。