```
```
...: plt.plot(Hom35Ret,':')
...: plt.ylabel('Hom35Ret')
...: plt.ylim(-0.13,0.10)
...: plt.title('\u53d8\u52a8\u6027\u80dc\u5229\u65f6\u95f4\u5e8f\u5217')
Out[9]: <matplotlib.text.Text at 0xaa92da0>
```
```

下面我们提取出35日动量指标预测正确时的收益率与预测失败时的收益率，并进行比较分析。如图28.4所示。

![](bbox=[0.11, 0.25, 0.95, 0.42])

![](bbox=[0.11, 0.42, 0.95, 0.58])

图 28.4 盈利与损失收益率的直方图

```
```
In [10]: loss=-Hom35Ret[Hom35Ret<0]

In [11]: plt.subplot(211)
 ....:  win.hist()
 ....:  plt.title('\u80dc\u5229\u65f6\u5386\u53f2\u65f6\u5e8f\u5217')
 ....:
 ....:  plt.subplot(212)
 ....:  loss.hist()
 ....:  plt.title('\u635f\u5931\u65f6\u5386\u53f2\u65f6\u5e8f\u5217')
Out[11]: <matplotlib.text.Text at 0xade84a8>
```
```

接着，我们计算两种收益率的平均值与分位数值。

```
```
In [12]: performance=pd.DataFrame({'win':win.describe(),\
 ...:          'loss':loss.describe()})
 ....:  performance
Out[12]:
            loss            win
count 136.000000 148.000000
mean    0.018514   0.018646
```
```