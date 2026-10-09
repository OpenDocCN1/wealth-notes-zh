# 9.3 Py thon 内置数据类型与操作

# 第 9 章 Py thon 标准库与数据操作

# 表 9.7 集合运算符

| 调用方法 (method) | 操作符运算 | 运算含义 |
| :--- | :--- | :--- |
| set1.union(set2) | set1 \| set2 | 集合 set1 与 set2 作并运算 |
| set1.intersection(set2) | set1 & set2 | 集合 set1 与 set2 作交运算 |
| set1.difference(set2) | set1 - set2 | 集合差运算，删去集合 set1 中与 set2 相同的元素 |
| set1.symmetric_difference(set2) | set1 \^ set2 | 集合对称差运算，返回集合 set1 与 set2 中除了两集合共有元素以外的所有元素 |
| set1.issubset(set2) | set1<= set2 | 如果集合 set1 为集合 set2 的子集，则返回 True；否则，返回 False |
| set1.issuperset(set2) | set1>= set2 | 如果集合 set2 为集合 set1 的子集，则返回 True；否则，返回 False |
| set1.isdisjoint(set2) |  | 如果集合 set2 和 set1 无交集，则返回 True，否则返回 False |

下面举一些集合运算的例子。

```
```text
In [1]: set1={20,30,5,6,7}
In [2]: set2={2,3,5,7,8}
·
#并运算
In [3]: set1.union(set2),set1|set2
Out[3]: ({2, 3, 5, 6, 7, 8, 20, 30}, {2, 3, 5, 6, 7, 8, 20, 30})

#交运算
In [4]: set1.intersection(set2),set1 & set2
Out[4]: ({5, 7}, {5, 7})

#差运算
In [5]: set1.difference(set2),set1 - set2
Out[5]: ({6, 20, 30}, {6, 20, 30})

#对称差运算
In [6]: set1.symmetric_difference(set2),set1 ^ set2
Out[6]: ({2, 3, 6, 8, 20, 30}, {2, 3, 6, 8, 20, 30})

#{1,2,3}是否属于{1,2,3}的子集判断
In [7]: {1,2,3}<= {1,2,3}
Out[7]: True

#{1,2,3}是否属于{1,2,3,'python'}真于集判断
In [8]: {1,2,3}<{1,2,3,'python'}
Out[8]: True

#包含关系判断
In [9]: {1,2,3,'python'}.issuperset({1,2,3})
Out[9]: True

#有无交集判断
In [10]: {5,6,7}.isdisjoint({})
Out[10]: True
```
```

108