意，在 ndarray 与 Python 中内置的 list、tuple 并不相同：在 Python 中，元素的数据类型可以不同；而在 ndarray 中，所有元素的数据类型必须相同。

首先，我们利用 NumPy 中的 array 函数创建一个一维 ndarray:

```
In [2]:data = [1, 2, 3, 4]
In [3]:arr = np.array(data)
    arr
Out[3]:array([1, 2, 3, 4])
```

等长序列组成的列表将会被转换为一个多维数组：

```
In [4]:data1 = [data, data]
In [5]:arr1 = np.array(data1)
    arr1
Out[5]:array([[1, 2, 3, 4],
             [1, 2, 3, 4]])
```

除了 np.array 函数，还有其他函数可以快速创建数组：

```
In [6]:np.zeros((3, 3))  #创建全 0 的数组
Out[6]:array([[ 0.,  0.,  0.],
             [ 0.,  0.,  0.],
             [ 0.,  0.,  0.]])
In [7]:np.ones((3, 3))  #创建全 1 的数组
Out[7]:array([[ 1.,  1.,  1.],
             [ 1.,  1.,  1.],
             [ 1.,  1.,  1.]])
In [8]:np.arange(1, 10, 2)  #创建 1~10 且差为 2 的等差数列
Out[8]:array([1, 3, 5, 7, 9])
In [9]:np.linspace(1, 10, 4)  #创建 1~10 且长度为 4 的等差数列
Out[9]:array([ 1.,  4.,  7., 10.])
```

# 3.1.2 ndarray 的数据类型

dtype 代表 ndarray 中元素的数据类型，数据类型有很多，对于新手来说，要全部记住这些数据类型是十分困难的。不过没有关系，在大部分数据分析工作中，我们只需要知道 float（浮点数）、int（整数）、bool（布尔值）、string_（字符串）、unicode_就可以了。若想了解更多的数据类型，则可以查阅 NumPy 的文档。

接下来举几个例子，代码如下：