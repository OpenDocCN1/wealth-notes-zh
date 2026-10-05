day：日期。

direction：当其取值为 All 时表示“汇总”，当其取值为 Sell 时表示“卖”，当其取值为 Buy 时表示“买”。

abnormal_code：异常波动类型。

abnormal_name：异常波动名称。

sales_depart_name：营业部名称。

rank：当其取值为 0 时表示汇总，当其取值为 1~5 时表示买一到买五，当其取值为 6~10 时表示卖一到卖五。

buy_value：买入金额。

buy_rate：买入金额占比（买入金额/市场总成交额）。

sell_value：卖出金额。

sell_rate：卖出金额占比（卖出金额/市场总成交额）。

net_value：净额（买入金额 - 卖出金额）。

amount：市场总成交额。

# 8.11.2 get_billboard_list() 函数的应用实例

创建一个 Python 3 文件，然后输入代码如下：

```
import pandas as pd
dataframe1 = get_billboard_list(stock_list=None,end_date = '2018-04-19',count =1)
dataframe1
```

单击工具栏中的 运行 按钮，运行结果如图 8.19 所示。

![](bbox=[0.118, 0.639, 0.848, 0.9])