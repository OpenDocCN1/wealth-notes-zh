# 从零开始学 Python 大数据与量化交易

只显示开盘价（open）的数据信息，代码如下：

```
import pandas as pd
dataframe1=get_price('000009.XSHE',start_date='2018-07-3',end_date='2018-09-10', frequency='daily')
dataframe1["open"]
```

注意：这里是中括号而不是小括号。

运行结果如图 8.13 所示。

![](bbox=[0.2, 0.235, 0.83, 0.475])

显示开盘价（open）、收盘价（close）和成交金额（money）的数据信息，代码如下：

```
dataframe1[["open","close","money"]]
```

运行结果如图 8.14 所示。

![](bbox=[0.155, 0.61, 0.88, 0.84])

显示第四条到第六条数据信息，代码如下：

```
dataframe1[3:6]
```