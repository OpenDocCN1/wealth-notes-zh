可以使用 dataframe.index 和 dataframe.columns 来查看 DataFrame 的行和列，dataframe.values 则以数组的形式返回 DataFrame 的元素：

```
```
print "DataFrame index:"
print df.index
print "DataFrame columns:"
print df.columns
print "DataFrame values:"
print df.values
```
```

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
```

DataFrame 也可以从值是数组的字典创建，但是各个数组的长度应相同：

```
```
d = {'one': [1., 2., 3., 4.], 'two': [4., 3., 2., 1.]}
df = DataFrame(d, index=['a', 'b', 'c', 'd'])
print df
one two
a 1.0 4.0
b 2.0 3.0
c 3.0 2.0
d 4.0 1.0
```
```

值非数组时，没有这一限制，并且缺失值置为 NaN：

```
```
d = [ {'a': 1.6, 'b': 2}, {'a': 3, 'b': 6, 'c': 9}]
df = DataFrame(d)
print df
a b c
0 1.6 2 NaN
1 3.0 6 9.0
```
```

在实际处理数据时，有时需要创建一个空的 DataFrame，方法如下：

```
```
df = DataFrame()
print df
Empty DataFrame
Columns: []
Index: []
```
```

另一种创建 DataFrame 的方法十分有用，那就是使用 concat 函数基于 Series 或者 DataFrame 创建一个 DataFrame：

```
```
a = Series(range(5))
b = Series(np.linspace(4, 20, 5))
```
```