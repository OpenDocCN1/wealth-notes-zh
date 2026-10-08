```
```python
    rhs[i] = x[i-1] * self.ldiag[i] + x[i] * self.cdiag[i] + x[i+1] * self.udiag[i]
    rhs[self.length - 1] = x[self.length - 2] * self.ldiag[self.length - 1] +
    x[self.length - 1] * self.cdiag[self.length - 1]
    return rhs
```
```

# ### 15.1.2 隐式格式求解

代码如下：

```
```python
for k in range(0, M):
    udiag = - np.ones(N-1) * rho
    ldiag = - np.ones(N-1) * rho
    cdiag = np.ones(N-1) * (1.0 + 2. * rho)
    mat = TridiagonalSystem(udiag, cdiag, ldiag)
    rhs = U[1:N,k]
    x = mat.solve(rhs)
    U[1:N, k+1] = x
    U[0][k+1] = 0.
    U[N][k+1] = 0.
```
```

先调用如下代码：

```
```python
# coding=utf-8
from CA_.PyCAL import *
from matplotlib import pylab
import seaborn as sns
import numpy as np
font.set_size(20)
from mpl_toolkits.mplot3d import Axes3D
from matplotlib import cm
class HeatEquation:
    def __init__(self, kappa, X, T, initialConston = lambda x:4.0*x*(1.0-x),
            boundaryConditionL = lambda x: 0, boundaryConditionR = lambda x:0):
        self.kappa = kappa
        self.ic = initialConston
        self.bcl = boundaryConditionL
        self.bcr = boundaryConditionR
        self.X = X
        self.T = T

def plotSurface(xGrids, yGrids, zGrids, title, xlabel, ylabel, zlabel):
    fig = pylab.figure(figsize = (16,10))
    ax = fig.add_subplot(1, 1, 1, projection = '3d')
    surface = ax.plot_surface(xGrids, yGrids, zGrids, cmap = cm.coolwarm)
    ax.set_xlabel(xlabel, fontdict = {"size":18})
    ax.set_ylabel(ylabel, fontdict = {"size":18})
    ax.set_zlabel(zlabel, fontdict = {"size":18})
    ax.set_title(title, fontproperties = font)
    fig.colorbar(surface, shrink = 0.75)

def plotLines(lines, x, title, xlabel, ylabel, legend):
    assert len(lines) == len(legend)
    pylab.figure(figsize = (12,6))
    for line in lines:
        pylab.plot(x, line)
    pylab.xlabel(xlabel, fontsize = 15)
    pylab.ylabel(ylabel, fontsize = 15)
    pylab.title(title, fontproperties = font)
```
```