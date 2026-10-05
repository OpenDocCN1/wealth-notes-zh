```
# 2.查看一维数组 Series 的统计特性
stocks.describe()
>> count        6.000000
mean    465.071667
std     491.183757
min     54.740000
25%     175.320000
50%     186.380000
75%     835.450000
max     1139.490000
dtype: float64
# 3.iloc 查询
stocks.iloc [0]
>> 54.74
# 4.loc 查询
stocks.loc['Facebook']
>> 181.86
```

跟 NumPy 一维数组 Array 一样，Pandas 的 Series 也支持向量运算，但只能与索引值相同的值相加。

代码如下：

```
# 1. 向量运算，向量相加
s1 = pd.Series([1,2,3,4],index= ['a','b','c','d'])
s2 = pd.Series([10,20,30,40], index=['a','b','e','f'])
s3 = s1 +s2
s3
>> a    11.0
b    22.0
c    NaN
d    NaN
e    NaN
f    NaN
dtype: float64
```

在数据分析过程中如果不希望出现空值 NaN，有 2 个办法：

+   （1）用 dropna() 方法来删除空值 NaN。