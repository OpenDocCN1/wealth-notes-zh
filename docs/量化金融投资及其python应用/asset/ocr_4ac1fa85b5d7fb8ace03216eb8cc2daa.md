# 第 15 章  偏微分方程隐式差分法的 Python 应用

本章将引入隐式差分算法来解决第 14 章显式格式的稳定性问题，主要内容包括：

(1) 隐式差分格式描述。
(2) 三对角矩阵求解。
(3) 使用 SciPy 加速算法实现三对角矩阵求解。

本章的初始化代码如下：

```
```python
from CAL.PyCAL import *
from matplotlib import pylab
import seaborn as sns
import numpy as np
np.set_printoptions(precision = 4)
font.set_size(20)
def initialCondition(x):
    return 4.0 * (1.0 - x) * x
```
```

# 15.1 隐式差分格式

与第 14 章类似，本章从差分格式的数字表述开始。隐式格式与显式格式的区别在于时间方向选择的基准点不同，显式格式选择 $k$，而隐式格式选择 $k + 1$。

这里得到一个迭代方程组：
$$- \rho U_{j-1, k+1} + (1 + 2 \rho) U_{j, k+1} - \rho U_{j+1, k+1} = U_{j, k}, \quad 1 \leq j \leq N - 1, \quad 0 \leq k \leq M - 1$$
其中 $\rho = \frac{\kappa \Delta t}{(\Delta x)^2}$。

```
```
N = 500          #x方向网格数
M = 500          #t方向网格数
T = 1.0
X = 1.0
xArray = np.linspace(0,X,N+1)
yArray = map(initialCondition, xArray)
starValues = yArray
U = np.zeros((N+1,M+1))
U[:,0] = starValues
dx = X / N
dt = T / M
kappa = 1.0
rho = kappa * dt / dx / dx
```
```