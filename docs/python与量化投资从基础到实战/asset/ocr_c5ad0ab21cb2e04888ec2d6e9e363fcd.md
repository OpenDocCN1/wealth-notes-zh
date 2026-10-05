# 3.2.6 DataFrame 按多列排序

可以使用 list 给出需要排序的列，同时给出是升序还是降序：

```
In [73]: import pandas as pd
    data = pd.DataFrame({'group': ['a','a','a','b','b','b','c','c','c'], 'ounces': [4, 3, 12, 6, 7.5, 8, 3, 5, 6]})
    data.sort_values(by=['group','ounces'], ascending=[False, True], inplace=True)
In [74]: data
Out[74]:
     group ounces
6    c      3.0
7    c      5.0
8    c      6.0
3    b      6.0
4    b      7.5
5    b      8.0
1    a      3.0
0    a      4.0
2    a      12.0
```

上述代码的作用就是先按照 group 降序排列，当 group 相同时再按照 ounces 升序排列。参数中的 inplace=True 直接将排序后的结果存在 data，即直接用排序好的数据覆盖原始数据。

# 3.2.7 DataFrame 去重

在大多数时候，在数据中会有重复的数据，在做分析前需要进行去重：

```
In [75]: data = pd.DataFrame({'k1': ['one'] * 3 + ['two'] * 4, 'k2': [3,2,1,3,3,4,4]})
In [76]: data
Out[76]:
    k1  k2
0  one  3
1  one  2
2  one  1
3  two  3
4  two  3
5  two  4
6  two  4
```