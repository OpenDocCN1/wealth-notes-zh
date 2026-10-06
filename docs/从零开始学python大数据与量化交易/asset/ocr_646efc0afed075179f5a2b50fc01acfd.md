# 从零开始学 Python 大数据与量化交易

前面显示的是一只股票的财务数据信息，如何显示两只股票或多只股票的财务数据信息呢？下面举例说明。

创建一个 Python3 文件，然后输入以下代码：

```
import pandas as pd
myq = query(valuation).filter(valuation.code.in_(['000001.XSHE','600000.XSHG','000009.XSHE']))
dataframe1 = get_fundamentals(myq,'2018-4-12')
dataframe1
```

注意：这里不能使用 in 操作，要使用 in_()函数。

单击工具栏中的 运行 按钮，运行结果如图 11.8 所示。

![](bbox=[0.12, 0.286, 0.91, 0.47])

# 图 11.8 显示两只股票或多只股票的财务数据信息

还可以设置用不同的财务数据条件，然后把满足条件的股票信息显示出来。创建一个 Python3 文件，然后输入以下代码：

```
import pandas as pd
dataframe1 = get_fundamentals(query(
    valuation
).filter(
    valuation.market_cap > 1000,
    valuation.pe_ratio < 10,
).order_by(
    # 按市值降序排列
    valuation.market_cap.desc()
).limit(
    # 最多返回 100 个
    100
), date='2018-4-12')
dataframe1
```

这里设置的条件是总市值大于 1000 亿元并且市盈率小于 10，选出数据后按总市值降序排列。另外，选出的股票最多显示 100 只，即如果满足条件的股票大于 100 只，也只显示前 100 只股票的财务信息。