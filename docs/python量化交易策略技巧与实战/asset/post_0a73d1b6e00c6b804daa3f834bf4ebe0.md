
在单元格中输入代码如下：

```
import numpy as np
numpy1 = np.array([[8,6,5.2],[3.5,-8,0.59]])
print("显示 Numpy 数组中所有元素：",numpy1)
print()
print("显示 Numpy 数组中第一行元素：",numpy1[0])
print()
print("显示 Numpy 数组中第一行中的第二个元素：",numpy1[0,1])
print()
print("显示 Numpy 数组中第二行中的第三元素：",numpy1[1,2])
print()
print("显示 Numpy 数组中第三列元素：",numpy1[:,2])
```

单击工具栏中的 运行 按钮，运行结果如图 6.16 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_162_0.png)

图 6.16 Numpy 数组索引运行结果

## 5) Numpy 数组运算

Numpy 数组运算，是指 Numpy 数组中元素的加、减、乘、除、乘方、最大值、最小值等运算。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。然后单击“新建”→“Python 3”命令，即可新建一个 Python 3 文件，并将其命名为“Numpy 数组运算”。

在单元格中输入代码如下：

```
import numpy as np
numpy1 = np.array([8,6,5.2])
numpy2 = np.array([3.5,-8,0.59])
print("数组的加法运算",numpy1+numpy2)
print("数组的减法运算",numpy1-numpy2)
print("数组的乘法运算",numpy1*numpy2)
print("数组的除法运算",numpy1/numpy2)
print("numpy1数组的乘方运算",numpy1**2)
#就是把数组的乘法运算得到的数，再加起来
print("数组的点乘运算",np.dot(numpy1,numpy2))
print("数组的大小比较",numpy1>=numpy2)
print("numpy1数组的最大值",numpy1.max())
print("numpy2数组的最小值",numpy2.min())
print("numpy1数组的和",numpy1.sum())
print("numpy1和numpy2数组的和",numpy1.sum()+numpy2.sum())
```

单击工具栏中的 运行 按钮，运行结果如图 6.17 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_163_0.png)

## 6）Numpy 数组复制

Numpy 数组复制分两种，即浅复制和深复制。

浅复制通过数组变量的复制完成，只复制数组的引用，这样对浅复制数组中的元素进行修改时，原数组中对应的元素也会被修改。

深复制通过数组对象 copy 的方法完成，是对储存内存进行复制。这样对深复制数组中的元素进行修改时，原数组中对应的元素不会改变。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。然后单击“新建”→“Python 3”命令，就可以新建一个 Python 3 文件，并将其命名为“Numpy 数组复制”。

在单元格中输入代码如下：

```
import numpy as np
numpy1 = np.array([8,6,5.2])
numpy2 = numpy1
print("浅复制数组中的数据：",numpy2)
numpy2[1] = -18.6    #修改浅复制数组中的数据
print("修改浅复制数组中的数据后：",numpy2)
print("原数组 numpy1 中的数据也被修改了：",numpy1)
print()
numpy3 = np.zeros(3)
numpy4 = numpy3.copy()
print("深复制数组中的数据：",numpy4)
numpy4[1] = -18.6    #修改深复制数组中的数据
print("修改深复制数组中的数据后：",numpy4)
print("原数组 numpy3 中的数据没有被修改：",numpy3)
```

单击工具栏中的 运行 按钮，运行结果如图 6.18 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_164_0.png)

图 6.18 Numpy 数组复制运行结果

### 6.1.2 Numpy 的矩阵对象

矩阵（Matrix）是一个按照长方形阵列排列的复数或实数集合，是高等代数中的常见工具，也常见于统计分析等应用数学学科。

Numpy 的矩阵对象与数组对象相似，主要的不同之处在于，矩阵对象的计算遵循矩阵数学运算规律，即矩阵的乘、转置、求逆等。需要注意的是，矩阵使用 matrix 函数创建。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。然后单击“新建”→“Python 3”命令，即可新建一个 Python 3 文件，并将其命名为“Numpy 的矩阵”。

在单元格中输入代码如下：

```
import numpy as np
numpy1=np.matrix([[2,6,8],[14,19,22]])
print("矩阵数据内容: ")
print(numpy1)
numpy2 = numpy1.T          #矩阵的转置
print("矩阵的转置后的数据内容: ")
print(numpy2)
print("矩阵的乘法: ")
print(numpy1*numpy2)
numpy3 = numpy1.I          #矩阵的求逆
print("矩阵的求逆: ")
print(numpy3)
```

单击工具栏中的 运行 按钮，运行结果如图 6.19 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_165_0.png)

# 图 6.19 Numpy 的矩阵运行结果

## 6.2 Pandas 库

Pandas 是基于 Numpy 构建的，可以使以 Numpy 为中心的应用变得更加简单。Pandas 提供了大量便捷处理数据的函数和方法，这也是使 Python 成为强大而高效的数据分析环境的重要因素之一。

Pandas 的数据结构主要有 3 种，分别是 Series、DataFrame 和 Panel。

Series 是一维数组，与 Numpy 中的一维 array 类似。二者与 Python 基本的数据结构 list 也很相近，其区别是：list 中的元素可以是不同的数据类型，而 array 和 Series 中只允许存储相同的数据类型，这样可以更有效地使用内存，提高运算效率。

DataFrame 是二维数组，非常接近于 Excel 电子表格或者类似 MySQL 数据库的形式。它的列称为 columns, 行称为 index, 也就是说数据的位置是通过 columns 和 index 来确定的。可以将 DataFrame 理解为 Series 的容器。

Panel 是三维的数组，可以理解为 DataFrame 的容器。

### 6.2.1 一维数组 Series

Series 由一组数据（各种 Numpy 数据类型），以及一组与之相关的标签数据（即索引）组成。仅由一组数据即可产生最简单的 Series，也可以通过传递一个 list 对象来创建一个 Series。需要注意的是，Pandas 会默认创建整型索引。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的 “策略研究” → “研究环境” 命令，打开研究环境页面。单击 “新建” → “文件夹” 命令，即可新建一个文件夹，然后将其命名为 “Pandas”。

双击 “Pandas” 文件夹，然后单击 “新建” → “Python 3” 命令，即可新建一个 Python 3 文件，并将其命名为 “一维数组 Series”，然后输入如下代码：

```
import pandas as pd
#创建最简单的 Series，Pandas 会默认创建整型索引
pandas1 = pd.Series(["Python","C","C++","C#","Java","VB","VC"])
print("显示 Series 中的内容: ")
print(pandas1)
print("只显示 Series 中的索引: ",pandas1.index)
print("只显示 Series 中的数据值: ",pandas1.values)
pandas2 = pd.Series(["Python","C","C++","C#","Java","VB","VC"],index=["a","b","c","d","e","f","g"])  #创建带有索引的 Series
print("显示 Series 中的内容: ")
print(pandas2)
```

单击工具栏中的 运行 按钮，运行结果如图 6.20 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_167_0.png)

图 6.20 一维数组 Series 运行结果

### 6.2.2 二维数组 DataFrame

DataFrame 是一个表格型的数据结构，它含有 一组有序的列，每一列的数据结构都是相同的，而不同列之间则可以是不同的数据结构（数值、字符、布尔值等）。以数据库进行类比，DataFrame 中的每一行是一个记录，是名称为 Index 的一个元素；而每一列则为一个字段，是这个记录的一个属性。DataFrame 既有行索引也有列索引，可以被看作由 Series 组成的字典（共用同一个索引）。

- 1) 创建 DataFrame

二维数组 DataFrame 可以使用各种输入创建，如列表、字典、一维数组系列 Series、Numpy ndarrays、另一个二维数组 DataFrame。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。双击“Pandas”文件夹，然后单击“新建”→“Python 3”命令，新建一个 Python 3 文件，并将其命名为“创建 DataFrame”，然后输入如下代码：

```
import pandas as pd
data = {"name": ["yahoo","google","facebook"], "marks": [200,400,800], "price": [9, 3, 7]}
dataframe1 = pd.DataFrame(data)
print("显示 dataframe1 中的数据: ")
```

```
print(dataframe1)
```

单击工具栏中的 运行 按钮，运行结果如图 6.21 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_168_0.png)

- 2）查看数据

下面利用 get_price()函数获得股票数据，得到的数据是 DataFrame 对象。

get_price()函数的语法格式如下：

```
get_price(security, start_date=None, end_date=None, frequency='daily', fields=None, skip_paused=False, fq='pre', count=None)
```

get_price()函数可以按天或者按分钟获取一只或者多只股票的行情数据，各项参数意义如下。
security: 一只股票代码或者一只股票代码的 list。
start_date: 开始时间，与参数 count 二选一，不可同时使用。需要注意的是，如果参数 count 和参数 start_date 都没有设置，则 start_date 生效值是 “2015-01-01”。取分钟数据时，时间可以精确到分钟，例如传入 datetime.datetime(2015, 1, 1, 10, 0, 0) 或者 “2015-01-01 10:00:00”。

> 提醒：当取分钟数据时，如果只传入日期，则日内时间是当日的 00:00:00。当取天数据时，传入的日内时间将被忽略。

end_date: 结束时间，默认值是 “2015-12-31”，包含此日期。需要注意的是，当取分钟数据时，如果 end_date 只有日期，则日内时间等同于 00:00:00，所以返回的数据是不包括 end_date 这一天的。

frequency: 单位时间长度，几天或者几分钟，默认为 daily，即表示 1 天。现在支持 “Xd”、“Xm”、“daily”（表示 1 天）、“minute”（表示 1 分钟），其中，X 是一个正整数，分别表示 X 天和 X 分钟。需要注意的是，当 X > 1 时，fields 只支持 “open” “close” “high” “low” “volume” “money” 这几个标准字段。

fields: 字符串 list，选择要获取的行情数据字段，默认是 None（表示 “open” “close” “high” “low” “volume” “money” 这几个标准字段）。参数 Fields 支持 SecurityUnitData 中的所有基本属性，包含 “open” “close” “low” “high” “volume” “money” “factor” “high_limit” “low_limit”

“avg” “pre_close” “paused”。

skip_paused： 是否跳过不交易日期（包括停牌、未上市或者退市后的日期）。如果不跳过，停牌时会使用停牌前的数据填充。上市前或者退市后数据都为 nan。需要注意的是，该参数默认为 False，即不跳过不交易日期。当该参数为 True 时，只能取一只股票的信息。

fq： 复权选项。参数值设为 pre，表示前复权，为默认设置；参数值设为 None，表示不复权，返回实际价格；参数值设为 post，表示后复权。

count： 与 start_date 二选一，不可同时使用。参数 count 表示数量，返回的结果集的行数，即表示获取 end_date 之前几个 frequency 的数据。

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的 “策略研究” → “研究环境” 命令，打开研究环境页面。双击 “Pandas” 文件夹，然后单击 “新建” → “Python 3” 命令，即可新建一个 Python 3 文件，并将其命名为 “查看数据”，然后输入如下代码：

```
import pandas as pd
dataframe1 = get_price('000009.XSHE',start_date='2018-04-03', end_date='2018-04-20', frequency='daily')
dataframe1
```

利用 get_price() 函数获得中国宝安（000009）每天的股票数据信息，开始时间是 2018 年 4 月 3 日，结束时间是 2018 年 4 月 20 日。默认情况下显示的数据信息是开盘价（open）、收盘价（close）、最低价（low）、最高价（high）、成交量（volume）、成交金额（money）。

单击工具栏中的 ▶ 运行 按钮，运行结果如图 6.22 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_169_0.png)

若需要只显示前 5 条数据信息，可以利用 head()函数，代码如下：

```
dataframe1.head()
```

单击工具栏中的 ▶ 运行 按钮，运行结果如图 6.23 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_170_0.png)

图 6.23 只显示前 5 条数据信息

若需要只显示后 5 条数据信息，可以利用 tail()函数，代码如下：

```
dataframe1.tail()
```

单击工具栏中的 ▶ 运行 按钮，运行结果如图 6.24 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_170_1.png)

图 6.24 只显示后 5 条数据信息

- 3）选择数据

成功登录 JoinQuant 聚宽量化交易平台后，单击导航栏中的“策略研究”→“研究环境”命令，打开研究环境页面。双击“Pandas”文件夹，然后单击“新建”→“Python 3”命令，新建一个 Python 3 文件，并将其命名为“选择数据”。

只显示开盘价（open）的数据信息，代码如下：

```
dataframe1["open"]
```

注意，这里的括号是中括号不是小括号。

单击工具栏中的 运行 按钮，运行结果如图 6.25 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_171_0.png)

图 6.25 只显示开盘价(open)的数据信息

显示开盘价（open）、收盘价（close）和成交金额（money）的数据信息，代码如下：

```
dataframe1[["open","close","money"]]
```

单击工具栏中的 运行 按钮，运行结果如图 6.26 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_171_1.png)

图 6.26 显示开盘价（open）、收盘价（close）和成交金额（money）的数据信息

显示第 4 条到第 6 条数据信息，代码如下：

```
dataframe1[3:6]
```

单击工具栏中的 运行 按钮，运行结果如图 6.27 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_172_0.png)

# 图 6.27 显示第 4 条到第 6 条数据信息

使用标签选取数据，语法格式如下：

```
dataframe1.loc[行标签,列标签]
dataframe1.loc['a':'b']      #选取 a 行到 b 行数据
dataframe1.loc[:, 'open']   #选取 open 列的数据
```

dataframe1.loc 的第一个参数是行标签，第二個参数为列标签（可选参数，默认为所有列标签），两个参数既可以是列表也可以是单个字符，如果两个参数都为列表则返回内容为 DataFrame，否则为 Series。

> 提示： loc 为 location 的缩写。

显示 2018 年 4 月 16 日的数据信息，代码如下：

```
dataframe1.loc["2018-04-16"]
```

单击工具栏中的 运行 按钮，运行结果如图 6.28 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_172_1.png)

# 图 6.28 显示 2018 年 4 月 16 日的数据信息

显示 2018 年 4 月 16 日的收盘价信息，代码如下：

```
dataframe1.loc["2018-04-16","close"]
```

单击工具栏中的 运行 按钮，运行结果如图 6.29 所示。
