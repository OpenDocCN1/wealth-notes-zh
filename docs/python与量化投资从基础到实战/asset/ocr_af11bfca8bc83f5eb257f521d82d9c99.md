# 3.2.12 连续型变量分组

有时我们把数值聚集在一起更有意义，例如，如果要为交通状况（路上的汽车数量）根据时间（分钟数据）建模，则具体的分钟可能不重要，而时段如上午、下午、傍晚、夜间、深夜能更好地表现预测结果，这样建模会更直观，也能避免过度拟合：

```
In [94]: cat=pd.cut(a.closePrice,bins)
        bins=[4,9,10,20,30]
In [95]: cat
Out[95]:
0      (9, 10]
1     (20, 30]
2     (20, 30]
3      (4, 9]
4      (4, 9]
5     (10, 20]
Name: closePrice, dtype: category
Categories (4, object): [(4, 9] < (9, 10] < (10, 20] < (20, 30]]
```

也可以对分组后的结果进行统计：

```
In [96]: pd.value_counts(cat)
Out[96]:
(20, 30]    2
(4, 9]      2
(10, 20]    1
(9, 10]     1
Name: closePrice, dtype: int64
```

也可以给定各个分组的标签并进行切分：

```
In [97]: group_names = ['low', 'Middle_1', 'Middle_2', 'high']
In [98]: pd.cut(a.closePrice, bins, labels=group_names)
Out[98]:
0    Middle_1
1        high
2        high
3         low
4         low
5    Middle_2
Name: closePrice, dtype: category
Categories (4, object): [low < Middle_1 < Middle_2 < high]
```