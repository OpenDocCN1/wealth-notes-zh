```
In [22]: ret=np.eye(3)
ret
Out[22]: array([[ 1., 0., 0.],
           [ 0., 1., 0.],
           [ 0., 0., 1.]])
```

图 9-6

```
In [33]: ret=np.mat('1 2 3 4 5 6 7 8 9')
ret
Out[33]: matrix([[1, 2, 3],
                [4, 5, 6],
                [7, 8, 9]])
```

图 9-7

```
In [33]: ret.shape ^ 1,1])
ret.size 4 5 6 7 8 9')
ret.sort
ret.squeeze
ret.std
Out[33] ret.strides
ret.sum
ret.swapaxes
ret.T
ret.take
In [ ], ret.trace
ret.t.
```

图 9-8

比如，用 T 属性获取转置矩阵，如图 9-9 所示。

```
In [6]: ret.T
Out[6]: matrix([[1, 4, 7],
               [2, 5, 8],
               [3, 6, 9]])
```

图 9-9

(3) 通过 NumPy 数组创建矩阵，如图 9-10 所示。

```
In [5]: ret=np.mat(np.arange(16).reshape(4, 4))
ret
Out[5]: matrix([[ 0, 1, 2, 3],
                [ 4, 5, 6, 7],
                [ 8, 9, 10, 11],
                [12, 13, 14, 15]])
```

图 9-10

# 9.1.3 随机数生成

关于随机数的生成方法是读者必须掌握的。NumPy.random 模块对 Python 的内置函数 random 进行了补充，增加了高效生成多种概率分布样本值的函数。举例：我们可以用 normal 得到一个标准正态分布，如图 9-11 所示。