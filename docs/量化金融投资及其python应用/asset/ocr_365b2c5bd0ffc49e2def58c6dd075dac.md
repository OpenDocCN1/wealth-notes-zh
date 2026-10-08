```
ax.set_xlabel("$x$", fontdict = {"size":18})
ax.set_ylabel(r"$\tau$", fontdict = {"size":18})
ax.set_zlabel(r"$U$", fontdict = {"size":18})
ax.set_title(u"热传导方程 $u_{\\tau\\tau}= u_{\\xx}$", fontproperties = font)
fig.colorbar(surface,shrink = 0.75)
```

运行上面的代码，得到如图 14-3 所示的图形。

![](bbox=[0.26, 0.22, 0.8, 0.51])

# 14.3  模块组装

就像在 13.2 节的二叉树建模中介绍的一样，本节以面向对象的方式重新组装分散的模块，以方便复用。首先是方程的描述：

```
class HeatEquation:
    def __init__(self, kappa, X, T, initialConstion = lambda x: 4.0 * x * (1.0 - x), boundaryConditionL = lambda x: 0, boundaryConditionR = lambda x:0):
        self.kappa = kappa
        self.ic = initialConstion
        self.bcl = boundaryConditionL
        self.bcr = boundaryConditionR
        self.X = X
        self.T = T
```

下面是显式差分格式的描述：

```
class ExplicitEulerScheme:
    def __init__(self, M, N, equation):
        self.eq = equation
```