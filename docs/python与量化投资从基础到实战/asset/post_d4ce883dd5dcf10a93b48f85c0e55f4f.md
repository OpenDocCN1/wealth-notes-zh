
首先，导入 numpy.random。为了更直观地展示随机数的生成过程，这里同时导入了 Matplotlib，以将结果可视化：

```
In [34]:import numpy.random as npr
    import matplotlib.pyplot as plt
    %matplotlib inline
```

例如，npr.rand 函数可以用来生成[0,1)的随机多维数组：

```
In [35]:npr.rand(3, 2)
Out[35]:array([[ 0.75531867,  0.96373173],
               [ 0.35314904,  0.33969993],
               [ 0.58071311,  0.74715197]])
```

稍微开动一下大脑，就可以将随机区间转化为[2,4):

```
In [36]:a = 2
        b = 4
        npr.rand(3, 2) * (b - a) + a
Out[36]:array([[ 2.97882682,  2.11641403],
               [ 2.17445139,  2.02036574],
               [ 3.05646641,  2.60940393]])
```

表 3-2 展示了生成简单随机数的函数及其参数和描述。

## 表 3-2

| 函数 | 参数 | 描述 |
|---|---|---|
| rand | d0, d1, ..., dn | 生成半开区间[0, 1)内的多维随机数 |
| randn | d0, d1, ..., dn | 生成来自标准正态分布的多个样本 |
| randint | low[, high, size] | 生成半开区间[low, high)内的随机整数 |
| choice | a[, size, replace, p] | 生成在给定的一维数组中的随机样本 |
| bytes | length | 生成随机字节 |

然后，对表 3-2 中部分函数生成的随机数进行可视化，效果如图 3-1 所示。

```
In [37]:size = 1000
        rn1 = npr.rand(size, 2)
        rn2 = npr.randn(size)
        rn3 = npr.randint(0, 10, size)
        rang = [0, 10, 20, 30, 40]
        rn4 = npr.choice(rang, size = size)
In [38]:fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(nrows = 2, ncols = 2,
figsize = (10, 10)
    ax1.hist(rn1, bins = 25, stacked = True)
    ax1.set_title('rand')
    ax1.set_ylabel('frequency')
    ax1.grid(True)
    ax2.hist(rn2, bins = 25)
    ax2.set_title('randn')
    ax2.grid(True)
    ax3.hist(rn3, bins = 25)
    ax3.set_title('randint')
    ax3.set_ylabel('frequency')
    ax3.grid(True)
    ax4.hist(rn4, bins = 25)
    ax4.set_title('choice')
    ax4.grid(True)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_82_0.png)

图 3-1

许多金融模型，例如 BSM 模型、跳跃扩散模型、平方根扩散模型等，都依赖于正态分布。我们可以通过生成相应的随机数，来将原本连续的金融模型离散化，从而进行近似模拟。在 npr.random 模块中内置了很多分布函数，这里限于篇幅不再全面展示，感兴趣的读者可以自行查阅 NumPy 的文档。

作为例子，我们将如下分布的随机数进行可视化。

+   （1）n=100、P=0.3 的二项分布。
+   （2）均值为 10、标准差为 2 的正态分布。
+   （3）自由度为 0.5 的卡方分布。
+   （4）λ 为 2 的泊松分布。

相应的代码如下：

```
In [39]:rn5 = npr.binomial(100, 0.3, size)
    rn6 = npr.normal(10, 20, size)
    rn7 = npr.chisquare(0.5, size)
    rn8 = npr.poisson(2.0, size)
In [40]:fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(nrows = 2, ncols = 2, figsize = (10, 10))
    ax1.hist(rn5, bins = 25)
    ax1.set_title('binomial')
    ax1.set_ylabel('frequency')
    ax1.grid(True)
    ax2.hist(rn6, bins = 25)
    ax2.set_title('normal')
    ax2.grid(True)
    ax3.hist(rn7, bins = 25)
    ax3.set_title('chisquare')
    ax3.set_ylabel('frequency')
    ax3.grid(True)
    ax4.hist(rn8, bins = 25)
    ax4.set_title('poisson')
    ax4.grid(True)
```

其可视化效果如图 3-2 所示。

## 3.2 Pandas 的使用

![](img/0b89c6ed12810914fc2fb80a08af1fae_84_0.png)

图 3-2

Pandas 是基于 NumPy 衍生出的一种工具，用于解决数据分析问题，它纳入了大量的库和一些标准的数据模型，提供了可用于高效操作大型数据集的工具，是使 Python 成为强大而高效的数据分析工具的重要因素之一。

对金融数据的大部分处理是依赖 Pandas 实现的。在本书中对 Pandas 的引入约定为：

```
import pandas as pd
```

一旦在代码中看到 pd，就是使用了 Pandas。

# Python 与量化投资：从基础到实战

## 3.2.1 Pandas 的数据结构

Pandas 的数据结构主要分为三种： Series（一维数组）、 DataFrame（二维的表格型数据结构）和 Panel（三维数组）。

### 1. Series

Series 指一维数组，与 NumPy 中的一维 Array 类似。 Series、Array 与 Python 基本的数据结构 List 也很相近，其区别是：在 List 中的元素可以是不同的数据类型，而在 Array 和 Series 中则只允许存储相同的数据类型，这样可以更有效地使用内存，提高运算效率。

Series 增加了对应的标签（label）以用于索引，可以包含 0 个或者多个任意数据类型的实体。其中，标签索引赋予了 Series 强大的存取元素功能。除通过位置外， Series 还允许通过索引标签进行元素存取：

```
In [41]: obj = pd.Series([40, 12, -3, 25])
In [42]: obj
Out[42]:
0    40
1    12
2    -3
3    25
```

Series 的字符串表现形式为：索引在左边，值在右边。因为在建立过程中没有指定索引，所以 Python 会自动为我们加入一个 0~n 的整数索引，我们可以通过数字获取具体位置上的元素：

```
In [43]: obj[0]
Out[43]: 40
```

可以通过 index 与 values 获取 Series 的索引与数据：

```
In [44]: obj.index
Out[44]: RangeIndex(start=0, stop=4, step=1)
In [45]: obj.values
Out[45]: array([40, 12, -3, 25], dtype=int64)
```

当然，也可以在 Series 建立时就指定索引：

```
In [46]: obj = pd.Series([40, 12, -3, 25], index=['a','b','c','d'])
```

```
In [47]: obj
Out[47]:
    a    40
    b    12
    c    -3
    d    25
```

可以直接通过索引获取数值：

```
In [48]: obj['c']
Out[48]: -3
```

对于 Series 的各种计算，其结果也会保留 index：

```
In [49]: obj[obj>15]
Out[49]:
    a    40
    d    25
```

在 Python 中一切皆对象，Series 也不例外。可通过 Tab 键补全查看一个 Series 的方法，即输入前几个字母，按下 Tab 键，代码如下：

```
In [50]: obj.describe()
Out[50]:
    count     4.000000
    mean     18.500000
    std      18.339393
    min      -3.000000
    25%       8.250000
    50%      18.500000
    75%      28.750000
    max      40.000000
```

```
In [51]: obj.mean()
Out[51]: 18.5
```

另外，Series 可以被转换为字典：

```
In [52]: obj.to_dict()
Out[52]: {'a': 40, 'b': 12, 'c': -3, 'd': 25}
```

### 2. DataFrame

DataFrame 指二维的表格型数据结构。在 DataFrame 有很多功能与 R 中的 data.frame

类似，我们可以将 DataFrame 理解为 Series 的容器，也就是说，在 DataFrame 中，多个 Series 共用了一个索引 index。

在以字典或 Series 的字典的结构构建 DataFrame 时，最外面的字典对应 DataFrame 的列，内嵌的字典及 Series 则是其中的每个值，例如：

```
In [53]: d = {'one' : pd.Series([1., 2., 3.], index=['a', 'b', 'c']),'two': pd.Series([1., 2., 3., 4.], index=['a', 'b', 'c', 'd'])} In [54]: df = pd.DataFrame(d) In [55]: df Out[55]:
    one two
a 1.0 1.0
b 2.0 2.0
c 3.0 3.0
d NaN 4.0
```

从字典的列表中构建 DataFrame 时，其中的每个字典代表的是每条记录（DataFrame 中的一行），字典中每个值对应的是这条记录的相关属性。

同时可以看到，当由多个 Series 组成 DataFrame 时，Pandas 会自动按照 index 对齐数据，如果某个 Series 的 index 缺失，则 Pandas 会将其自动填写为 np.nan。

## 3.2.2 Pandas 输出设置

在 Pandas 中可以通过 set_option 设置 Pandas 的输出格式，例如最多显示的行数、列数等：

```
In [56]: import pandas as pd
In [57]: pd.set_option("display.max_rows",1000)
In [58]: pd.set_option("display.max_columns",20)
In [59]: pd.set_option('precision',7)
In [60]: pd.set_option('large_repr', 'truncate')
```

## 3.2.3 Pandas 数据读取与写入

Pandas 可以方便地读取本地文件如 csv、txt、xlsx 等，例如：

```
In [61]: a=pd.read_csv('closeprice.csv',encoding='gbk',dtype={'ticker':
str})
```

```
In [62]: a
Out[62]:
```

其结果截图如图 3-3 所示。

|  | Unnamed: 0 | ticker | secShortName | tradeDate | closePrice |
|---|---|---|---|---|---|
| 0 | 0 | 000001 | 平安银行 | 2017-06-20 | 9.12 |
| 1 | 1 | 000002 | 万科A | 2017-06-20 | 21.03 |
| 2 | 2 | 000004 | 国农科技 | 2017-06-20 | 27.03 |
| 3 | 3 | 000005 | 世纪星源 | 2017-06-20 | 5.45 |
| 4 | 4 | 000006 | 渭南业A | 2017-06-20 | 8.87 |
| 5 | 5 | 000007 | 全新好 | 2017-06-20 | 15.87 |

图 3-3

我们可自行输入 pd.read_，使用 Tab 自动完成功能查看 Pandas 可以读取的数据类型。

同理，可以使用 to_，将 DataFrame 输出到文件中：

```
In [63]: a.to_excel('closeprice.xls')
```

## 3.2.4 数据集快速描述性统计分析

在读入数据后，我们需要对数据做基本的统计分析，代码如下：

```
In [64]: import pandas as pd
In [65]: data = pd.read_csv('closeprice.csv',encoding='gbk')
In [66]: data.describe().T
Out[66]:
        count  mean        std     min     25%    50%    75%    max
Unnamed: 0 6.0   2.5000000 1.8708287 0.00 1.2500 2.500 3.75 5.00
ticker     6.0   4.1666667 2.3166067 1.00 2.5000 4.500 5.75 7.00
closePrice 6.0  14.5616667 8.2950550 5.45 8.9325 12.495 19.74 27.03
```

可以加入参数 include ='all'，这样 describe 就会展示全部列的统计结果。当然，我们在这里没必要对 ticker 进行均值统计。df.describe()会统计出各列的计数、平均数、方差、最小值、最大值及 quantile 数值。df.info()会展示数据类型、行列数和 DataFrame 占用的内存：

# 3.2.5 根据已有的列建立新列

本节将根据现有的代码列，加入该股票代码对应的行业列：

```
In [67]: data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 5 columns):
Unnamed: 0      6 non-null int64
ticker          6 non-null int64
secShortName    6 non-null object
tradeDate       6 non-null object
closePrice      6 non-null float64
dtypes: float64(1), int64(2), object(2)
memory usage: 312.0+ bytes
```

```
In [68]: a=pd.read_csv('closeprice.csv',encoding='gbk')
In [69]: a
Out[69]:
Unnamed: 0  ticker  secShortName  tradeDate  closePrice
0  0  1  平安银行  2017-06-20  9.12
1  1  2  万科 A  2017-06-20  21.03
2  2  4  国农科技  2017-06-20  27.03
3  3  5  世纪星源  2017-06-20  5.45
4  4  6  深振业 A  2017-06-20  8.87
5  5  7  全新好  2017-06-20  15.87
```

使用 map 函数映射一个字典即可：

```
In [70]: b={1:'银行',2:'房地产',4:'医药生物',5:'房地产',6:'采掘',7:'休闲服务',8:'机械设备'}
In [71]: a['ind']=a.ticker.map(b)
In [72]: a
Out[72]:
Unnamed: 0  ticker  secShortName  tradeDate  closePrice  ind
0  0  1  平安银行  2017-06-20  9.12  银行
1  1  2  万科 A  2017-06-20  21.03  房地产
2  2  4  国农科技  2017-06-20  27.03  医药生物
3  3  5  世纪星源  2017-06-20  5.45  房地产
4  4  6  深振业 A  2017-06-20  8.87  采掘
5  5  7  全新好  2017-06-20  15.87  休闲服务
```
