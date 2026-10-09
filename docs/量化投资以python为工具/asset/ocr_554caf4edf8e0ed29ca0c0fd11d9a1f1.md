# 第11章 常用第三方库：Pandas与数据处理

Pandas 是建立在 Numpy 之上，并与其他第三方库在科学运算环境中有较好兼容性的 Python 包。其最大特色在于提供灵活直观的数据结构来处理关联数据和有标签的数据。

Pandas 提供了两大数据结构：一维结构的 Series 类型数据和二维结构的 DataFrame 数据。这两大数据结构都有数据标签这一独特性质，数据标签能够对不同变量的数据按照相同数据标签进行汇总，为多个变量的特定数据元素提取、变量合并等操作分析提供了极大的便利。因而，Pandas 被广泛运用于金融、统计以及社会科学领域的数据处理。

# 11.1 Series 类型数据

Series 类型的数据由一列数据及与之对应的标签（索引，位于数据的左侧）两部分组成。Series 对象本质上是一个 NumPy 数组，因此 NumPy 的数组处理函数同样适用于 Series 对象。每个 Series 对象实际上都由两个数组组成，具有 index 和 values 两大属性。

+   - index: 保存标签信息，是从 NumPy 数组继承的 Index 对象；
- values: 保存值，是一维 NumPy 数组对象。

Series 对象看起来由两列数据构成，实则是一维数据结构。

# 11.1.1 Series 对象的创建

Series 对象的创建通过 Series 类的构造函数 Series( ) 函数来实现。在创建 Series 对象之前，要先导入 pandas 包，我们一般使用简写 pd 来代指 pandas。创建一个 Series 对象时，可以同时为其 index 属性和 values 属性赋值，如果没有对这两个属性赋值，则创建一个空的 Series 对象。

```
In [1]: import pandas as pd
... : import numpy as np
... :
In [2]: S1=pd.Series()
In [3]: S1
Out[3]: Series([], dtype: float64)
```

Series 对象一般有如下几种创建方式。

+   - 同时指定 index 属性和 values 属性的具体值