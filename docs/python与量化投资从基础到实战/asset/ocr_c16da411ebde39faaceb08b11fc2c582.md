# 第3章

# Python 进阶

# 3.1 NumPy 的使用

NumPy 是高性能科学计算和数据分析的基础包，是本章接下来要讲解的 Pandas、Scikit-Learn、StatModels 等库的构建基础。NumPy 的主要功能在第 1 章中已经有所介绍，对于一般的金融数据分析而言，其大部分功能可由更高级的 Pandas 实现。我们并不需要在学习 NumPy 上花费过多精力，但是回过头来深入学习 NumPy 可以帮助我们更好地理解面向数组编程的思维方式。

在本书中对 NumPy 的引入约定为：

```
In [1]: import numpy as np
```

因此，一旦在代码中看到 np，就是使用了 NumPy。

# 3.1.1 多维数组 ndarray

NumPy 的主要对象是 ndarray，该对象是一个快速、灵活的大数据容器。在此需要注