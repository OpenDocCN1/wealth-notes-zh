
可以使用 `dataframe.index` 和 `dataframe.columns` 来查看 DataFrame 的行和列，`dataframe.values` 则以数组的形式返回 DataFrame 的元素：

```python
print "DataFrame index:"
print df.index
print "DataFrame columns:"
print df.columns
print "DataFrame values:"
print df.values
```

```
DataFrame index:
Index([u'r', u'd', u'a'], dtype= 'object')
DataFrame columns:
Index([u'two', u'three'], dtype= 'object')
DataFrame values:
[[nan nan]
 [4.0 nan]
 [1.0 nan]]
```

DataFrame 也可以从值是数组的字典创建，但是各个数组的长度应相同：

```python
d = {'one': [1., 2., 3., 4.], 'two': [4., 3., 2., 1.]}
df = DataFrame(d, index=['a', 'b', 'c', 'd'])
print df
```

```
   one  two
a  1.0  4.0
b  2.0  3.0
c  3.0  2.0
d  4.0  1.0
```

值非数组时，没有这一限制，并且缺失值置为 NaN：

```python
d = [ {'a': 1.6, 'b': 2}, {'a': 3, 'b': 6, 'c': 9}]
df = DataFrame(d)
print df
```

```
     a    b    c
0  1.6  2.0  NaN
1  3.0  6.0  9.0
```

在实际处理数据时，有时需要创建一个空的 DataFrame，方法如下：

```python
df = DataFrame()
print df
```

```
Empty DataFrame
Columns: []
Index: []
```

另一种创建 DataFrame 的方法十分有用，那就是使用 `concat` 函数基于 Series 或者 DataFrame 创建一个 DataFrame：

```python
a = Series(range(5))
b = Series(np.linspace(4, 20, 5))
df = pd.concat([a, b], axis=1)
print df
```

```
   0    1
0  0  4.0
1  1  8.0
2  2  12.0
3  3  16.0
4  4  20.0
```

其中的 `axis=1` 表示按列合并，`axis=0` 表示按行合并，并且 Series 都处理成一列，所以这里如果选 `axis=0`，将得到一个 10×1 的 DataFrame。下面这个例子展示了如何将 DataFrame 按行合并成一个大的 DataFrame:

```python
df = DataFrame()
index = ['alpha', 'beta', 'gamma', 'delta', 'eta']
for i in range(5):
    a = DataFrame([np.linspace(i, 5 * i, 5)], index=[index[i]])
    df = pd.concat([df, a], axis=0)
print df
```

```
        0     1     2     3     4
alpha  0.0   0.0   0.0   0.0   0.0
beta   1.0   2.0   3.0   4.0   5.0
gamma  2.0   4.0   6.0   8.0  10.0
delta  3.0   6.0   9.0  12.0  15.0
eta    4.0   8.0  12.0  16.0  20.0
```

### 5.3.2 DataFrame 数据的访问

首先，再次强调一下 DataFrame 是以列作为操作基础的，全部操作都可以视为先从 DataFrame 里取一列，成为一个 Series，再从这个 Series 中取元素。可以用 `dataframe.column_name` 选取列，也可以用 `dataframe[]` 选取列，前一种方法只能选取一列，而后一种方法可以选取多列。若 DataFrame 没有列名，在 `[]` 中可以使用非负整数，也就是通过“下标”选取列；若有列名，则必须使用列名选取，另外 `dataframe.column_name` 在没有列名的时候是无效的：

```python
print df[1]
print type(df[1])
df.columns = ['a', 'b', 'c', 'd', 'e']
print df['b']
print type(df['b'])
print df.b
print type(df.b)
print df[['a', 'd']]
print type(df[['a', 'd']])
```

```
alpha    0.0
beta     2.0
gamma    4.0
delta    6.0
eta      8.0
Name: 1, dtype: float64
<class 'pandas.core.series.Series'>
alpha    0.0
beta     2.0
gamma    4.0
delta    6.0
eta      8.0
Name: b, dtype: float64
<class 'pandas.core.series.Series'>
alpha    0.0
beta     2.0
gamma    4.0
delta    6.0
eta      8.0
Name: b, dtype: float64
<class 'pandas.core.series.Series'>
     a     d
alpha  0.0  0.0
beta   1.0  4.0
gamma  2.0  8.0
delta  3.0  12.0
eta    4.0  16.0
<class 'pandas.core.frame.DataFrame'>
```

以上代码使用了 `df.columns`（即 `dataframe.columns`）为 DataFrame 赋列名，可以看到，单独取一列出来时，其数据结构显示的是 Series，而取两列及两列以上的结果仍然是 DataFrame。访问特定的元素可以像 Series 一样使用下标或者索引：

```python
print df['b'][2]
print df['b']['gamma']
```

```
4.0
4.0
```

若需要选取行，可以使用 `dataframe.iloc` 按下标选取，或者使用 `dataframe.loc` 按索引选取：

```python
print df.iloc[1]
print df.loc['beta']
```

```
a    1.0
b    2.0
c    3.0
d    4.0
e    5.0
Name: beta, dtype: float64
a    1.0
b    2.0
c    3.0
d    4.0
e    5.0
Name: beta, dtype: float64
```

选取行还可以使用切片的方式或者布尔类型的向量：

```python
print "Selecting by slices:"
print df[1:3]
bool_vec = [True, False, True, True, False]
print "Selecting by boolean vector:"
print df[bool_vec]
```

Selecting by slices:

|   | a   | b   | c   | d   | e   |
|---|-----|-----|-----|-----|-----|
| beta   | 1.0 | 2.0 | 3.0 | 4.0 | 5.0 |
| gamma  | 2.0 | 4.0 | 6.0 | 8.0 | 10.0 |

Selecting by boolean vector:

|   | a   | b   | c   | d   | e   |
|---|-----|-----|-----|-----|-----|
| alpha  | 0.0 | 0.0 | 0.0 | 0.0 | 0.0 |
| gamma  | 2.0 | 4.0 | 6.0 | 8.0 | 10.0 |
| delta  | 3.0 | 6.0 | 9.0 | 12.0 | 15.0 |

将行列组合起来选取数据：

```python
print df[['b', 'd']].iloc[[1, 3]]
print df.iloc[[1, 3]][['b', 'd']]
print df[['b', 'd']].loc[['beta', 'delta']]
print df.loc[['beta', 'delta']][['b', 'd']]
```

|   | b   | d   |
|---|-----|-----|
| beta   | 2.0 | 4.0 |
| delta  | 6.0 | 12.0 |

|   | b   | d   |
|---|-----|-----|
| beta   | 2.0 | 4.0 |
| delta  | 6.0 | 12.0 |

|   | b   | d   |
|---|-----|-----|
| beta   | 2.0 | 4.0 |
| delta  | 6.0 | 12.0 |

|   | b   | d   |
|---|-----|-----|
| beta   | 2.0 | 4.0 |
| delta  | 6.0 | 12.0 |

如果不需要访问特定行列，而只是需要访问某个特殊位置的元素，`dataframe.iat` 和 `dataframe.at` 是最快的方式，它们分别用于使用下标和索引进行访问：

```python
print df.iat[2, 3]
print df.at['gamma', 'd']
```

```
8.0
8.0
```

`dataframe.ix` 可以混合使用索引和下标进行访问，唯一需要注意的是行列内部应一致，不可以同时使用索引和下标访问，否则会得到意外的结果：

```python
print df.ix['gamma', 4]
print df.ix[['delta', 'gamma'], [1, 4]]
print df.ix[[1, 2], ['b', 'e']]
print "Unwanted result:"
print df.ix[['beta', 2], ['b', 'e']]
print df.ix[[1, 2], ['b', 4]]
```

```
10.0
   b     e
delta  6.0  15.0
gamma  4.0  10.0
   b     e
beta   2.0  5.0
gamma  4.0  10.0
Unwanted result:
   b     e
beta   2.0  5.0
2      NaN  NaN
   b     4
beta   2.0  NaN
gamma  4.0  NaN
```

### 练习题

对本章中的例题数据，使用 Python 重新操作一遍。

## 第 6 章 pandas 在金融数据处理中的应用

在第 5 章中介绍了如何创建并访问 pandas 的 Series 和 DataFrame 类型的数据，本章将介绍如何对 pandas 数据进行操作，掌握这些操作之后，就可以处理大多数的数据了。

首先导入本章中使用的模块：

```python
import numpy as np
import pandas as pd
from pandas import Series, DataFrame
```

为了查看数据方便一些，要设置输出屏幕的宽度：

```python
pd.set_option('display.width', 200)
```

### 6.1 创建数据结构的方式

数据结构的创建不只是第 5 章中介绍的标准方式。例如，可以创建一个以日期为元素的 Series：

```python
dates = pd.date_range('20170101', periods=5)
print dates
```

```
DatetimeIndex(['2017-01-01', '2017-01-02', '2017-01-03', '2017-01-04', '2017-01-05'],
             dtype='datetime64[ns]', freq='D')
```

将这个日期 Series 作为索引赋给一个 DataFrame：

```python
df = pd.DataFrame(np.random.randn(5, 4), index=dates, columns=list('ABCD'))
print df
```

|           |    A |    B |    C |    D |
|:----------|-----:|-----:|-----:|-----:|
| 2017-01-01 | -1.25593 | 1.308361 | -1.119820 | -0.486524 |
| 2017-01-02 | -0.155901 | -0.096743 | 0.452969 | -0.246108 |
| 2017-01-03 | -0.014116 | -0.745056 | 0.480347 | -2.677346 |
| 2017-01-04 | -1.864881 | -1.246534 | -0.222377 | -0.104438 |
| 2017-01-05 | 0.363992 | 0.597859 | 1.897772 | 0.643010 |

只要是能转换成 Series 的对象，都可以用于创建 DataFrame：

```python
df2 = pd.DataFrame({'A': 1., 'B': pd.Timestamp('20170214'), 'C': pd.Series(1.6,
                          index=list(range(4))), 'D': np.array([4] * 4, dtype='int64'),
                    'E': 'hello pandas!'})
print df2
```

```
     A          B    C    D              E
0  1.0  2017-02-14  1.6    4  hello pandas!
1  1.0  2017-02-14  1.6    4  hello pandas!
2  1.0  2017-02-14  1.6    4  hello pandas!
3  1.0  2017-02-14  1.6    4  hello pandas!
```

### 6.2 数据的查看

在多数情况下，数据并不由分析数据的人员生成，而是通过数据接口、外部文件或者其他方式获取。这里通过优矿量化投资平台（注意：后面所有的代码需要在优矿平台环境下运行）的数据接口获取一份数据作为示例：

```python
stock_list = ['000001.XSHE', '000002.XSHE', '000568.XSHE', '000625.XSHE', '000768.XSHE', '600028.XSHG', '600030.XSHG', '601111.XSHG', '601390.XSHG', '601998.XSHG']
raw_data = DataApi.MktEqudGet(secID = stock_list, beginDate = '20170101', endDate = '20170131', pandas = 1)
df = raw_data[['secID', 'tradeDate', 'secShortName', 'openPrice', 'highestPrice', 'lowestPrice', 'closePrice', 'turnoverVol']]
```

以上代码获取了 2017 年 1 月全部交易日内 10 只股票的日行情信息，首先看一下数据的大小：

```python
print df.shape
```

```
(180, 8)
```

可以看到有 180 行，表示获取了 180 条记录，每条记录有 8 个字段，现在预览一下数据，用 `dataframe.head()` 和 `dataframe.tail()` 可以查看数据的开头 5 行和结尾 5 行，若需要改变行数，可在括号内指定：

```python
print "Head of this DataFrame:"
print df.head()
print "Tail of this DataFrame:"
print df.tail(3)
```

Head of this DataFrame:

| secID | tradeDate | secShortName | openPrice | highestPrice | lowestPrice | closePrice | turnoverVol |
|---|---|---|---|---|---|---|---|
| 000001.XSHE | 2017-01-03 | 平安银行 | 9.11 | 9.18 | 9.09 | 9.16 | 45984049 |
| 000001.XSHE | 2017-01-04 | 平安银行 | 9.15 | 9.18 | 9.14 | 9.16 | 44932953 |
| 000001.XSHE | 2017-01-05 | 平安银行 | 9.17 | 9.18 | 9.15 | 9.17 | 34437291 |
| 000001.XSHE | 2017-01-06 | 平安银行 | 9.17 | 9.17 | 9.11 | 9.13 | 35815420 |
| 000001.XSHE | 2017-01-09 | 平安银行 | 9.13 | 9.17 | 9.11 | 9.15 | 36108157 |

Tail of this DataFrame:

| secID | tradeDate | secShortName | openPrice | highestPrice | lowestPrice | closePrice | turnoverVol |
|---|---|---|---|---|---|---|---|
| 601998.XSHG | 2017-01-24 | 中信银行 | 6.91 | 6.97 | 6.87 | 6.93 | 24045549 |
| 601998.XSHG | 2017-01-25 | 中信银行 | 6.91 | 6.95 | 6.86 | 6.92 | 19225348 |
| 601998.XSHG | 2017-01-26 | 中信银行 | 6.92 | 7.02 | 6.90 | 6.98 | 28835194 |

`dataframe.describe()` 提供了 DataFrame 中纯数值数据的统计信息：

```python
print df.describe()
```
