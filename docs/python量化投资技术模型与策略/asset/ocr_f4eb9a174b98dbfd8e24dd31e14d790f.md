# 3.2 SciPy

SciPy是基于NumPy的，提供了更多的科学计算功能，比如线性代数、优化、积分、插值、信号处理等。

# 1.文件读写

目前在国内Matlab仍然非常流行，Matlab使用的数据格式通常是.mat文件。对此，Scipy.io包提供了可以导入导出.mat文件的接口，这样，Python和Matlab的协同工作就变得非常容易了。示例代码如下所示：

```
In[2]: from scipy import io as spio
In[3]: import numpy as np
In[4]: a=np.arange(10)
In[5]: spio.savemat('a.mat',{'a':a})
In[8]: data = spio.loadmat('a.mat', struct_as_record=True)
In[10]: data['a']
Out[10]: array([[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]])
```

# 2.线性代数运算

在SciPy中，线性代数运算使用的是scipy.linalg。

scipy.linalg.det()可用于计算矩阵的行列式，示例代码如下：

```
In[17]: from scipy import linalg
In[18]: m=np.array([[1,2],[3,4]])
```