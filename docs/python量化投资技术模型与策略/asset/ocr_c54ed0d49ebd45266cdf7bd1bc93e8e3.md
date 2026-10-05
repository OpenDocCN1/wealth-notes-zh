```
In[20]: linalg.det(m)
Out[20]: -2.0
```

# 3.优化和拟合

求解最大值最小值之类的问题即为优化问题，在SciPy中，

scipy.optimization提供了最小值、曲线拟合等算法。示例代码如下：

```
import numpy as np
from scipy import optimize
import matplotlib.pyplot as plt

def f(x):
    return x**2+20*np.sin(x)

x=np.arange(-10,10,0.1)

plt.plot(x,f(x))
```

由图3-1中可以看到，对应的最小值的横坐标大约是-2。

我们可以用暴力穷举法来计算最小值，代码如下：

```
In[29]: grid=(-10,10,0.1)
In[30]: x_min=optimize.brute(f,(grid,))
In[31]: x_min
Out[31]: array([-1.42754883])
```