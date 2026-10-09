```
# 累积收益率表现
```

```
In [7]: cumStock=np.cumprod(1+CBRet[SmaRet.index[0]:])-1
     ....: cumTrade=np.cumprod(1+SmaRet)-1
     ....: cumdata=pd.DataFrame({ 'cumTrade' :cumTrade,\
     ....: 'cumStock':cumStock})
     ....: cumdata.iloc[-6: ,:]
Out [7]:
       cumStock cumTrade
Date
2015-04-23  0.839080 -0.100261
2015-04-24  0.785441 -0.100261
2015-04-27  0.842912 -0.129223
2015-04-28  0.938697 -0.083964
2015-04-29  0.904215 -0.083964
2015-04-30  0.854406 -0.083964
```

```
# 绘制累积收益率图
In [8]: plt.plot(cumStock, label = "cumStock", color = 'k')
     ....: plt.plot(cumTrade, label = "cumTrade", color = 'r', linestyle = ':')
     ....: plt.title("简单均线交易策略的累积收益率")
     ....: plt.legend()
Out [8]: <matplotlib.legend.Legend at 0x1e8fae9e7b8>
```

如图 30.7 所示，可以看出简单均线交易策略的累计收益率较小。接下来，计算 10 日简单均线交易策略买卖点预测的准确率。

![](bbox=[0.04, 0.51, 0.88, 0.82])

```
# 求买卖点预测准确率
In [9]: SmaRet[SmaRet ==(-0)] = 0
     ....: smaWinrate=len(SmaRet[SmaRet>0])/len(SmaRet[SmaRet!=0])
     ....: smaWinrate
Out [9]: 0.3157894736842105
```