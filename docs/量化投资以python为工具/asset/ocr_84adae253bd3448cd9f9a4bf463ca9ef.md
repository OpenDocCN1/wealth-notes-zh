```
= p_{k+2} \times \alpha + \left[ p_{k+1} \times \alpha + \mathrm{EWMA}_{t=k} \times (1-\alpha) \right] \times (1-\alpha)
= p_{k+2} \times \alpha + p_{k+1} \times \alpha \, (1-\alpha) + \mathrm{EWMA}_{t=k} \times (1-\alpha)^2
```

依此类推，我们可以得出之后各期加权移动平均数。总的来说，当 $t \ge k + 1$ 时，有：

```
$$\mathrm{EWMA}_{t} = \alpha \left[ p_{t} + p_{t-1} \times (1-\alpha) + \cdots + p_{k+1}(1-\alpha)^{t-k-1} \right] + \mathrm{EWMA}_{t=k} \times (1-\alpha)^{t-k}$$
```

式中，$\mathrm{EWMA}_{t=k} = \frac{p_1+p_2+\cdots+p_k}{k}$ 。在公式中，我们可以看到加权移动平均数是权重 $(1-\alpha)$ 的指数函数，因此这种计算移动平均数的方法被称为指数加权移动平均。而且根据公式还可以看出，股价的指数加权移动平均数 $\mathrm{EWMA}_t$ 是过去所有期的价格 $(p_1, p_2, \cdots, p_t)$ 之加权平均，从当期往过去价格之权重呈现指数递减现象。若将各期指数加权移动平均数用折线图绘制出来，则可以得到股价的指数移动平均曲线。

还是以青岛啤酒股票为例，计算该股票收盘价的指数移动平均。再把指数移动平均用折线图绘制出来，就可以得到股价的指数移动平均曲线。如图 30.4 所示。

```
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
```

```
In [6]: plt.rcParams['font.sans-serif'] = ['SimHei']
....: plt.plot(Close[4:],label="Close",color='k')
....: plt.plot(Ema5[4:],label="Ema5",\
....:      color='g',linestyle='--')
....: plt.title("各周期移动平均线与股价走势对照")
....: plt.ylim(35,50)
....: plt.legend()
Out[6]: <matplotlib.legend.Legend at 0x1d52bed9908>
```