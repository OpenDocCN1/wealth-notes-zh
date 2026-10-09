.... tradeRet=ret*tradSig

#求出买卖交易合并的收益率
In [13]: tradeRet=ret*tradSig

+   - 绘制三种交易收益率的时序图，展现收益率的分布情况，如图 29.4 所示。

```
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

```

![](bbox=[0.03, 0.42, 0.9, 0.79])

# **图 29.4  RSI 策略收益率时序图**

+   - 计算信号点预测准确率情况，求预测正确时的平均收益率与预测失败时的平均收益率。

```
```python
In [15]: def strat(tradeSignal,ret):
....:     indexDate=tradeSignal.index
....:     ret=ret[indexDate]
....:     tradeRet=ret*tradeSignal
```

```