```
```
     ...: momen35.head()
Out[1]:
Date
2014-02-20    -0.71
2014-02-21    -0.83
2014-02-24    -1.15
2014-02-25    -0.91
2014-02-26    -0.75
Name: Close, dtype: float64
```
```

（2）结合35日动量值的取值情况来判断买卖点，35日动量释放的买卖点信号用 signal 表示。

```
```
# 当35日动量值为负值时，signal 取值为-1，表示卖出；
# 当35日动量值为非负值时，signal 取值为1，表示买入；

In [2]: signal=[]
     ...: for i in momen35:
     ...:     if i>0:
     ...:         signal.append(1)
     ...:     else:
     ...:         signal.append(-1)
     ...:
In [3]: signal=pd.Series(signal,index=momen35.index)
     ...: signal.head()
Out[3]:
Date
2014-02-20    -1
2014-02-21    -1
2014-02-24    -1
2014-02-25    -1
2014-02-26    -1
dtype: int64
```
```

（3）根据买卖点制定买入和卖出交易，并计算收益率。

```
```
In [4]: tradeSig=signal.shift(1)
In [5]: ret=Close/Close.shift(1)-1
In [6]: Mom35Ret=(ret*tradeSig).dropna()
     ...: Mom35Ret[:5]
Out[6]:
Date
2014-02-21     0.021858
2014-02-24     0.065642
2014-02-25     0.017937
2014-02-26    -0.016743
2014-02-27     0.013473
dtype: float64
```
```

（4）35日动量指标交易策略评价。