```
# 替换索引
In [24]: df.loc[:, 'E'] = np.arange(0, len(df))

In [25]: df
Out[25]:
         B          C          D
2016-01-01  0 -0.312846  0.000000
2016-01-02  1  1.064430  0.598022
2016-01-03  2  0.000000 -0.417211
2016-01-04  3  1.541127 -0.968151
2016-01-05  4  0.125405 -1.496913
2016-01-06  5  0.950862  0.452035
```

这里只列举几种方法，替换的方法就是选择到对应的数据并赋予新值即可。

+   - 重置索引：

如果对 DataFrame 对象 df 调用 reindex() 函数，将返回一个新的对象，该对象的 index 和列名由 reindex() 函数传入的参数设定。如果新对象的某个索引值或列名不存在于原对象 df 中，则引入缺失值。具体见下面的例子：

```
In [197]: new_index=pd.date_range('20150102', periods=7)

In [203]: df.reindex(new_index, columns=list('ABCD'))
Out[203]:
         A          B          C          D
2016-01-02 NaT  1  1.064430  0.598022
2016-01-03 NaT  2  0.000000 -0.417211
2016-01-04 NaT  3  1.541127 -0.968151
2016-01-05 NaT  4  0.125405 -1.496913
2016-01-06 NaT  5  0.950862  0.452035
2016-01-07 NaT NaT  NaT        NaT
2016-01-08 NaT NaT  NaT        NaT
```

# 11.2.5  DataFrame 的运算

+   - 简单运算

在分析数据时，不可避免地要对数据进行运算。当对两个数据集进行算术运算时，遇到的核心问题有两个：一是两个数据集之间如何进行匹配进而运算；二是如何处理不匹配的数据。Pandas 的两种数据类型（Series 与 DataFrame）比较有特色的部分是 index（DataFrame 还多了列名 columns），因此两个数据集最直接的匹配方式是按照 index 与 columns 进行匹配运算。如果两个数据集的加减乘除运算分别用“+”、“-”、“*”、“/”来实现，那么 Series 与 Series 之间是 index 匹配运算，无法匹配的用 NaN 进行填充；Series 与 DataFrame 之间是 Series 的 index 与 DataFrame 的 columns 之间匹配，运算的方式是将 DataFrame 每一行都与 Series 进行匹配运算，无法匹配的用 NaN 填充；DataFrame 与 DataFrame 之间是同时对 index 与 columns 进行匹配，匹配成功的元素进行运算，无法匹配的用 NaN 填充。

```
In [1]: import pandas as pd
```