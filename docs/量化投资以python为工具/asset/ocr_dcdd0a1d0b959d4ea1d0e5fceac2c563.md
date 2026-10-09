# 28.4 编写动量函数 momentum()

动量值一般采用差法求得，为了计算动量值方便，此处编写一个简单的动量函数 momentum()，该函数有价格 price 和期数 period 两个参数，函数定义如下：

```
```python
def momentum(price,period):
    lagPrice=price.shift(period)
    momen=price-lagPrice
    momen= momen.dropna()
    return(momen)
```
```

通过调用 momentum() 函数即可求动量值。

```
```python
In [8]: def momentum(price,period):
         ...:     lagPrice=price.shift(period)
         ...:     momen=price-lagPrice
         ...:     momen= momen.dropna()
         ...:     return(momen)
         ...
In [8]: momentum(Close,5).tail(n=5)
Out[8]:
Date
2015-04-22    0.98
2015-04-23    0.46
2015-04-24   -0.25
2015-04-27    0.49
2015-04-28   -0.29
Name: Close, dtype: float64
```
```

# 28.5 万科股票 2015 年走势及 35 日动量线

为了直观了解动量指标的含义和具体用法，以中期动量值 35 日动量为例，如图 28.2 所示绘制出万科股票 2015 年的蜡烛图和 35 日动量线。

观察图 28.2 中万科收盘价格曲线和 35 日动量线，可以看出动量线在 0 值下方时，万科股票价格大致处于下跌趋势。当动量线在 0 值以上时，股票价格走势整体处于上升趋势。此外，动量线的走势和蜡烛线的价格走势方向大致类似，但动量线变化的趋势可能会比蜡烛图的走势提前。

```
```python
# 计算 35 日动量值
In [1]: momen35=momentum(Close,35)
```
```

在绘制 K 线图上的，可以使用 candlePlot() 函数，但是此函数不能增加子图。下面，我们对 candlePlot() 函数进行修改和补充，再定义一个新的函数 candleLinePlots()，该函数返回的图形中包含两个子图，第一个子图为蜡烛图，第二个子图为线形图。在定义函数时，使用了不定长参数 *kwargs。函数定义如下：

```
```python
In [1]: import pandas as pd
         ...: import matplotlib.pyplot as plt
         ...: from matplotlib.dates import DateFormatter, WeekdayLocator,\
```
```