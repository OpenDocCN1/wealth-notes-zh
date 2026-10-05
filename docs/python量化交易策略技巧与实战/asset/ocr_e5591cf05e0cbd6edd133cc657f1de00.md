```
import pandas as pd
dataframe1 = get_fundamentals( query(valuation).filter(
valuation.pb_ratio < 1.2 ,
valuation.market_cap > 5000
)
, date='2018-08-01')
dataframe1['code']
```

valuation 为市值数据表名；查询条件为两个，分别为市净率小于 1.2、总市值大于 5 000 亿元；查询时间为 2018 年 8 月 1 日。

单击工具栏中的 运行 按钮，运行结果如图 9.14 所示。

![](bbox=[0.14, 0.321, 0.876, 0.52])

# 9.4.2 市销率选股技巧

市销率为股票价格与每股销售收入之比。市销率越小，通常被认为其投资价值越高。市销率的计算公式如下：

市销率=股价 ÷ 每股销售额

在诸多基本分析的工具中，市销率是常用的参考指标之一。一般情况下，对成熟期的企业来说，通常使用市盈率来估值；而对尚未盈利的高成长性企业来说，使用市销率来估值更为可靠。

# 市销率选股实例

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。双击“量化选股”文件夹，然后单击“新建”→“Python 3”命令，新建一个 Python 3 文件，并将其命名为“市销率选股”，然后输入如下代码：

```
import pandas as pd
dataframe1 = get_fundamentals( query(valuation).filter(
```