strike 为期权行权价。

spot 为标的现价。

本例只考虑看涨期权。

```
# 设置基本参数
ttm = 3.0
ttSteps = 25
r = 0.03
d = 0.02
sigma = 0.2
strike = 100.0
spot = 100.0
```

这里用作例子的二叉树称为 Jarrow-Rudd 树，其中：

$$up = e^{(r-d-0.5\sigma^2)} dt + \sigma \sqrt{dt}$$
$$down = e^{(r-d-0.5\sigma^2)} dt - \sigma \sqrt{dt}$$
$$upProb ability = 0.5$$

Python 代码如下：

```
dt = ttm / ttSteps
up = math.exp((r - d - 0.5 * sigma * sigma) * dt + sigma * math.sqrt(dt))
down = math.exp((r - d - 0.5 * sigma * sigma) * dt - sigma * math.sqrt(dt))
discount = math.exp(-r * dt)
```

在本例中，这个树的深度为 16 层 (时间节点数 +1)，第 i 层节点与第 i+1 层节点有以下关系式：

$$lattice[i+1][j+1] = lattice[i][j] * up$$
$$lattice[i+1][0] = lattice[i][0] * down$$

因此，构造二叉树的 Python 代码如下：

```
# 构造二叉树
lattice = np.zeros((ttSteps+1, ttSteps+1))
lattice[0][0] = spot
for i in range(ttSteps):
    for j in range(i+1):
        lattice[i+1][j+1] = up * lattice[i][j]
    lattice[i+1][0] = down * lattice[i][0]
```

绘图代码如下：

```
pylab.figure(figsize = (12, 8))
pylab.plot(lattice[ttSteps])
pylab.title(u'二叉树到期时刻标的价格分布', fontproperties = font, fontsize = 20)
```

最后得到如图 13-1 所示的图形。

```
# 在节点上计算 payoff
def call_payoff(spot):
    global strike
    return max(spot - strike, 0.0)

pylab.figure(figsize = (12, 8))
```