# 表 16-1 精确解

| | price | delta | gamma | vega | rho | theta |
|---|---|---|---|---|---|---|
| 1 | 26.761 844 | 0.749 694 | 0.007 11 | 71.103 19 | 241.037 549 | -3.832 439 |

固定时间方向网格数为 3000,分别计算不同 S 网格数情形下的结果:

```
xSteps = range(50,300,10)
finiteResult = []
for xStep in xSteps:
    model = BSModel(100.0, 0.05, 0.2)
    payoff = CallOption(105.0)
    scheme = BSMscheme(model, payoff, 5.0, 3000, xStep)
    scheme.roll_back()
    interg = CubicNaturalSpline(np.exp(scheme.xArray), scheme.C[:, -1])
    price = interp(100.0)
    finiteResult. append(price)
```

绘制收敛图的代码如下:

```
anyRes = [analyticPrice['price'][i]] * len(xSteps)
pylab.figure(figsize = (16,8))
pylab.plot(xSteps, finiteResult, '-', marker = 'o', markersize = 10)
pylab.plot(xSteps, anyRes, '--')
pylab.legend([u'隐式差分格式', u'解析解(欧式)'], prop = font)
pylab.xlabel(u'价格方向网格步数', fontproperties = font)
pylab.title(u'Black - Scholes - Merton 有限差分法', fontproperties = font, fontsize = 20)
```

利用上述代码可以画出如图 16-3 所示的收敛图。

![](bbox=[0.14, 0.53, 0.84, 0.8])

# 练习题

对本章中的例题数据,使用 Python 重新操作一遍。