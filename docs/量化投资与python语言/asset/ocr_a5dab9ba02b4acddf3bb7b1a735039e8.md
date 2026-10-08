```
In [23]: ret.values
Out[23]: array([11.45, 2.67, 36.45, 0.4])
In [24]: ret.index
Out[24]: RangeIndex(start=0, stop=4, step=1)
```

# 图 10-2

我们也可以自己进行设置，创建一个对数据进行标记的索引。
举例如图 10-3 所示：

```
In [25]: stockData=pd.Series([3246.22, 3255.78, 3229.13, 3245.22], index=["open", "high", "low", "close"])
              stockData
Out[25]: open     3246.22
high     3255.78
low      3229.13
close    3245.22
dtype: float64
```

# 图 10-3

可以看出，这和 NumPy 相比，可以通过索引的方式选取 Series 的单个或一组值。如图 10-4 所示。

```
In [26]: stockData['open']
Out[26]: 3246.2199999999998
In [27]: stockData["high", "close"]
Out[27]: high    3255.78
close   3245.22
dtype: float64
```

# 图 10-4

我们还可以把 Series 看作是一个有定长的有序字典，因为它本身就是一个索引值到数据值的映射。

从图 10-5 可以看出，Python 语言的互动性很强，而且只要读者对这门语言感兴趣，就可以很自然地猜到怎么去写程序，不断试错，并且还可以百度。

```
In [28]: ["volume" in stockData]
Out[28]: False
```

# 图 10-5

最后，如果数据被存放在一个 Python 字典中，也是可以直接通过这个字典创建Series，如图 10-6 所示。

```
In [29]: sdata={'open': 3246, 'high': 3255, 'low': 3229, 'close': 3245}
ret=pd.Series(sdata)
ret
Out[29]: close    3245
high     3255
low      3229
open     3246
dtype: int64
```

# 图 10-6