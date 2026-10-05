# 4.shuffle

shuffle可以随机打乱一个数组，并且改变此数组本身的排列。示例代码如下：

```
In[34]: x=np.arange(10)
In[35]: x
Out[35]: array([0,  1,  2,  3,  4,  5,  6,  7,  8,  9])
In[36]: np.random.shuffle(x)
In[37]: x
Out[37]: array([5,  9,  3,  4,  6,  1,  2,  8,  0,  7])
```

# 5.Permutation

Permutation用于返回一个打乱后的数组值，但是并不会改变传入的参数数组本身。示例代码如下：

```
In[40]: x=np.arange(10)
In[41]: y=np.random.permutation(x)
In[42]: x
Out[42]: array([0,  1,  2,  3,  4,  5,  6,  7,  8,  9])
In[43]: y
Out[43]: array([9,  6,  4,  8,  2,  1,  0,  7,  3,  5])
```

除了以上所讲的函数之外，NumPy还提供了其他大量的随机数生成函数。所有的随机数生成函数都包含在numpy.random模块中。

# 6.二项式分布