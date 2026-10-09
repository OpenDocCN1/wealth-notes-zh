2014-01-06      3.75      -0.04    0.00    0.04
2014-01-07      3.77       0.02    0.02    0.00
2014-01-08      3.80       0.03    0.03    0.00
2014-01-09      3.79      -0.01    0.00    0.01

（3）接下来用简单平均数计算交通银行收盘价 6 日的上涨力度 SMUP 和下跌力度 SMDOWN，然后计算 6 日 RSI 的值。

```
In [5]: SMUP=[]
...: SMDOWN=[]
...: for i in range(6,len(upPrc)+1):
...:     SMUP.append(np.mean(upPrc.values[(i-6):i],dtype=np.float32))
...:     SMDOWN.append(np.mean(downPrc.values[(i-6):i],dtype=np.float32))
...: 
# 计算 6 日 RSI 的值
...: rsi6=[100+SMUP[i]/(SMUP[i]+SMDOWN[i]) for i in range(0,len(SMUP))]

In [6]: indexRsi=indexprc[5:]
...: Rsi6=pd.Series(rsi6,index=indexRsi)
...: Rsi6.head()
Out[6]:
Date
2014-01-10      42.857141
2014-01-13      61.538465
2014-01-14      66.666665
2014-01-15      46.153845
2014-01-16      30.000001
dtype: float64
```

（4）对 6 日 RSI 的值进行描述性统计分析。如图 29.1 所示。

```
In [7]: Rsi6.describe()
Out[7]:
count      327.000000
mean       51.742495
std        27.787794
min        0.000000
25%        30.000001
50%        51.298706
75%        74.547477
max        100.000000
dtype: float64

In [8]: UP=pd.Series(SMUP,index=indexRsi)
...: DOWN=pd.Series(SMDOWN,index=indexRsi)
...: plt.rcParams['font.sans-serif'] = ['SimHei']
...: plt.subplot(411)
...: plt.plot(BOCBCIgp,'k')
...: plt.xlabel('date')
...: plt.ylabel('Close')
...: plt.title('RSI & ​...​')
...: 
...: plt.subplot(412)
...: plt.plot(UP,'b')
```