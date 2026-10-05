# 6.2.3 三维数组 Panel

获得股票数据 get_price()函数，如果获得的是多只股票的数据，则返回 panel 对象。可通过 panel[列标,行标,股票代码]获取数据。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。双击“Pandas”文件夹，然后单击“新建”→“Python 3”命令，即可新建一个 Python 3 文件，并将其命名为“三维数组 Panel”，然后输入如下代码：

```
import pandas as pd
panel1 = get_price(['000001.XSHE','000009.XSHE'],start_date='2018-09-3', end_date='2018-09-10', frequency='daily')
panel1
```

单击工具栏中的 运行 按钮，运行结果如图 6.39 所示。

图 6.37 利用函数 mean()计算所有列的平均值

图 6.38 利用函数 mean()计算行的平均值