```
self.dt = self.eq.T / M
self.dx = self.eq.X / N
self.U = np.zeros((N + 1, M + 1))
self.xArray = np.linspace(0,self.eq.X,N + 1)
self.U[:,0] = map(self.eq.ic, self.xArray)
self.rho = self.eq.kappa * self.dt / self.dx / self.dx
self.M = M
self.N = N

def roll_back(self):
    for k in range(0, self.M):
        for j in range(1, self.N):
            self.U[j][k + 1] = self.rho * self.U[j - 1][k] + (1. - 2 * self.rho) * self.U[j][k] + self.rho * self.U[j + 1][k]
    self.U[0][k + 1] = self.eq.bcl(self.xArray[0])
    self.U[N][k + 1] = self.eq.bcr(self.xArray[-1])

def mesh_grids(self):
    tArray = np.linspace(0, self.eq.T, M + 1)
    tGrids, xGrids = np.meshgrid(tArray, self.xArray)
    return tGrids, xGrids
```

有了以上的部分，现在整个过程可以简单地通过初始化一行关于 roll_back 的调用完成：

```
ht = HeatEquation(1.,.,1.,)
scheme = ExplicitEulerScheme(2500,25, ht)
scheme.roll_back()
```

可以获取与之前相同的图形，如图 14-4 所示。

![](bbox=[0.24, 0.56, 0.82, 0.85])

图 14-4 热传导方程