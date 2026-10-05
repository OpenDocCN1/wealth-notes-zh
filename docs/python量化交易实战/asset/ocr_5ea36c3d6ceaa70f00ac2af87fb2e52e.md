简介：两日 K 线模式，分多头吞噬和空头吞噬，以多头吞噬为例，第一日为阴线，第二日为阳线，第一日的开盘价和收盘价在第二日开盘价和收盘价之内，但不能完全相同。

```
函数：def CDLENGULFING(open, high, low, close)
```

# 20. 函数名：CDLEVENINGDOJISTAR

名称：Evening Doji Star（十字暮星）

简介：三日 K 线模式，基本模式为暮星，第二日收盘价和开盘价相同，预示顶部反转。

```
函数：def CDLEVENINGDOJISTAR(open, high, low, close, penetration=0)
```

# 21. 函数名：CDLEVENINGSTAR

名称：Evening Star（暮星）

简介：三日 K 线模式，与晨星相反，上升趋势中，第一日为阳线，第二日价格振幅较小，第三日为阴线，预示顶部反转。

```
函数：def CDLEVENINGSTAR(open, high, low, close, penetration=0)
```

# 22. 函数名：CDLGAPSIDESIDEWHITE

名称：Up/Down-gap side-by-side white lines（向上/下跳空并列阳线）

简介：二日 K 线模式，上升趋势向上跳空，下跌趋势向下跳空，第一日与第二日有相同的开盘价，实体长度差不多，则趋势持续。

```
函数：def CDLGAPSIDESIDEWHITE(open, high, low, close)
```

# 23. 函数名：CDLGRAVESTONEDOJI

名称：Gravestone Doji（墓碑十字/倒 T 十字）

简介：一日 K 线模式，开盘价与收盘价相同，上影线长，无下影线，预示底部反转。

```
函数：def CDLGRAVESTONEDOJI(open, high, low, close)
```

# 24. 函数名：CDLHAMMER

名称：Hammer（锤头）

简介：一日 K 线模式，实体较短，无上影线，下影线大于实体长度两倍，处于下跌趋势底部，预示反转。

```
函数：def CDLHAMMER(open, high, low, close)
```

# 25. 函数名：CDLHANGINGMAN

名称：Hanging Man（上吊线）

简介：一日 K 线模式，形状与锤子类似，处于上升趋势的顶部，预示着趋势反转。

```
函数：def CDLHANGINGMAN(open, high, low, close)
```