
# 13.2.2 二叉树类型描述

正像 13.1 节描述的那样，任意的二叉树只要描述 3 个方面的特征就可以，所以本节设计的 Tree Traits 类只需通过它的静态成员返回这 3 个特征：

- (1) up，返回上升的比例。
- (2) down，返回下降的比例。
- (3) upProbability，返回上升的概率。

下面的类定义了 Jarrow-Rudd 树的类型描述：

```python
class JarrowRuddTraits:
    @staticmethod
    def up(tree):
        return math.exp((tree.r - tree.d - 0.5 * tree.v * tree.v) * tree.dt + tree.v * math.sqrt(tree.dt))

    @staticmethod
    def down(tree):
        return math.exp((tree.r - tree.d - 0.5 * tree.v * tree.v) * tree.dt - tree.v * math.sqrt(tree.dt))

    @staticmethod
    def upProbability(tree):
        return 0.5
```

这里再给出 Cox-Ross-Rubinstein 树的描述：

```python
class CRRTraits:
    @staticmethod
    def up(tree):
        return math.exp(tree.v * math.sqrt(tree.dt))

    @staticmethod
    def down(tree):
        return math.exp(-tree.v * math.sqrt(tree.dt))

    @staticmethod
    def upProbability(tree):
        return 0.5 + 0.5 * (tree.r - tree.d - 0.5 * tree.v * tree.v) * tree.dt / tree.v / math.sqrt(tree.dt)
```

## 13.2.3 偿付函数

偿付函数很简单，是一元函数，输入为标的价格，输出为偿付收益，对于看涨期权来说就是

$$pay = \max (S-K,0)$$

代码如下：

```python
def pay_off(spot):
    global strike
    return max(spot - strike, 0.0)
```

## 13.2.4 组装

把上面3个模块组装起来，现在整个调用过程变得十分清晰，最后的结果和13.1节二叉树算法的结果是完全一致的。

```python
testTree = BinomialTree(spot, r, d, tSteps, ttm, sigma, JarrowRuddTraits)
testTree.roll_back(pay_off)
print('二叉树价格: %.4f' % testTree.lattice[0][0])
# 输出: 二叉树价格: 14.2663
```

下面用本节的算法框架来测试二叉树的收敛性。这里我们用来进行比较的算法即前面给出的 Jarrow-Rudd 树以及 Cox-Ross-Rubinstein 树：

```python
stepSizes = range(25, 500, 25)
jrRes = []
crrRes = []
for tSteps in stepSizes:
    # Jarrow - Rudd 树的结果
    testTree = BinomialTree(spot, r, d, tSteps, ttm, sigma, JarrowRuddTraits)
    testTree.roll_back(pay_off)
    jrRes.append(testTree.lattice[0][0])
    # Cox - Ross - Rubinstein 树的结果
    testTree = BinomialTree(spot, r, d, tSteps, ttm, sigma, CRRTraits)
    testTree.roll_back(pay_off)
    crrRes.append(testTree.lattice[0][0])
```

可以将两种二叉树算法随着步数的增加逐渐向真实值收敛的过程用图形展示出来：

```python
anyRes = [BSMPrice(1, strike, spot, r, d, sigma, ttm, rawOutput=True)[0]] * len(stepSizes)
pylab.figure(figsize=(16, 8))
pylab.plot(stepSizes, jrRes, '-', marker='o', markersize=10)
pylab.plot(stepSizes, crrRes, '-', marker='d', markersize=10)
pylab.plot(stepSizes, anyRes, '---')
pylab.legend(['Jarrow - Rudd', 'Cox - Ross - Rubinstein', '解析解'], prop=font)
pylab.xlabel('二叉树步数', fontproperties=font)
pylab.title('二叉树算法收敛性测试', fontproperties=font, fontsize=20)
```

运行上面的代码，得到如图13-3所示的图形。

也可以用图形展示两种算法的误差随着步长下降的过程。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_154_0.png)

图 13-3 二元树算法收敛性测试

```python
jrErr = np.array(jrRes) - np.array(anyRes)
crrErr = np.array(crrRes) - np.array(anyRes)
jrErr = np.log10(np.abs(jrErr))
crrErr = np.log10(np.abs(crrErr))
```

绘图代码如下：

```python
pylab.figure(figsize=(16, 8))
pylab.plot(stepSizes, jrErr, '-.', marker='o', markersize=10)
pylab.plot(stepSizes, crrErr, '-.', marker='d', markersize=10)
pylab.xlabel('二元树步数', fontproperties=font)
pylab.ylabel('误差(log)', fontproperties=font)
pylab.title('二元树算法误差分布测试', fontproperties=font, fontsize=20)
```

运行上面的代码，得到如图 13-4 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_154_1.png)

图 13-4 二元树算法误差分布测试

# 13.3 美式期权定价的二叉树算法

既然已经有解析算法了，为什么还要用二叉树算法来求解呢？当然，如果只是普通欧式期权定价问题，二叉树算法就是多此一举的做法。但是由于二叉树天然的回溯特性，使得它特别适合处理有提前行权结构的期权产品。本节以美式期权为例说明二叉树算法的应用。

美式期权的行权结构在二叉树下处理起来特别简单，只需在每个节点上做以下比较：

```python
lattice[i][j] = max(ExerciseValue, EuropeanValue)
```

这里的 ExerciseValue 就是立即行权的价值，EuropeanValue 为对应节点的欧式期权价值。为了实现上面的比较，需要扩展原先的算法，这可以通过 Python 的类继承在原先的类上添加新功能来实现：

```python
class ExtendBinomialTree(BinomialTree):
    def roll_back_american(self, payOff):
        # 节点计算并回溯
        for i in range(self.tSteps, 0, -1):
            for j in range(i, 0, -1):
                if i == self.tSteps:
                    europeanValue = self.discount * (self.upProbability * payOff(self.lattice[i][j]) + self.downProbability * payOff(self.lattice[i][j-1]))
                else:
                    europeanValue = self.discount * (self.upProbability * self.lattice[i][j] + self.downProbability * self.lattice[i][j-1])
                # 处理美式期权的行权结构
                exerciseValue = payOff(self.lattice[i-1][j-1])
                self.lattice[i-1][j-1] = max(europeanValue, exerciseValue)
```

下面使用同样的参数测试美式期权算法的结果：

```python
stepSizes = range(25, 500, 25)
jrRes = []
crrRes = []
for tSteps in stepSizes:
    # Jarrow - Rudd 树的结果
    testTree = ExtendBinomialTree(spot, r, d, tSteps, ttm, sigma, JarrowRuddTraits)
    testTree.roll_back_american(pay_off)
    jrRes.append(testTree.lattice[0][0])
    # Cox - Ross - Rubinstein 树的结果
    testTree = ExtendBinomialTree(spot, r, d, tSteps, ttm, sigma, CRRTraits)
    testTree.roll_back_american(pay_off)
    crrRes.append(testTree.lattice[0][0])
```

画出美式期权价格的收敛图，美式期权价格始终高于欧式期权价格，符合预期。

```python
anyRes = [BSMPrice(1, strike, spot, r, d, sigma, ttm, rawOutput=True)[0]] * len(stepSizes)
pylab.figure(figsize=(16, 8))
pylab.plot(stepSizes, jrRes, '-', marker='o', markersize=10)
pylab.plot(stepSizes, crrRes, '-', marker='d', markersize=10)
pylab.plot(stepSizes, anyRes, '--')
pylab.legend(['Jarrow - Rudd(美式)', 'Cox - Ross - Rubinstein(美式)', '解析解(欧式)'], prop=font)
pylab.xlabel('二叉树步数', fontproperties=font)
pylab.title('二叉树算法美式期权', fontproperties=font, fontsize=20)
```

得到如图 13-5 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_156_0.png)

## 练习题

对本章中的例题数据，使用 Python 重新操作一遍。

# 第14章 偏微分方程显式差分法的Python应用

本章以热传导方程为例，介绍有关偏微分方程的以下问题：

- (1) 偏微分方程的初边值问题。
- (2) 利用差分格式将偏微分方程离散化。
- (3) 显式差分格式。
- (4) 显式差分格式的条件稳定性。

## 14.1 热传导方程

本节使用热传导方程作为偏微分方程的例子：

$$u_t - \kappa u_{xx} = 0, \quad 0 \leqslant x \leqslant 1 \quad (14\text{-}1)$$
$$u(x,0) = 4x(1-x), \quad 0 \leqslant x \leqslant 1 \quad (14\text{-}2)$$
$$u(0,\tau) = 0, \quad \tau \geqslant 0 \quad (14\text{-}3)$$
$$u(1,\tau) = 0, \quad \tau \geqslant 0 \quad (14\text{-}4)$$

其中, $\kappa$ 称为热传导系数。

式(14-2)称为方程的初值条件(initial condition)，式(14-3)和式(14-4)称为方程的边值条件(boundaries condition)。这里使用 Dirichlet 条件。

可以看一下初值条件的形状：

```python
from matplotlib import pylab
import seaborn as sns
import numpy as np
from CaL.PyCaL import *
font.set_size(20)

def initialCondition(x):
    return 4.0 * (1.0 - x) * x

xArray = np.linspace(0, 1.0, 50)
yArray = map(initialCondition, xArray)
pylab.figure(figsize=(12, 6))
pylab.plot(xArray, yArray)
pylab.xlabel('$x$', fontsize=15)
pylab.ylabel('$f(x)$', fontsize=15)
pylab.title('一维热传导方程初值条件', fontproperties=font)
```

运行以上代码，得到如图 14-1 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_158_0.png)

## 14.2 显式差分格式

这里的基本思想是用差分格式替换对应的微分形式，并且期盼两种格式的“误差”在网格足够密的情况下会趋于 0。分别在时间方向以及空间方向对式(14-1)做差分：

$$\frac { u ( x , \tau + \Delta \tau ) - u ( x , \tau ) } { \Delta \tau } = \kappa \frac { u ( x + \Delta x , \tau ) - 2 u ( x , \tau ) + u ( x - \Delta x , \tau ) } { ( \Delta x ) ^ { 2 } }$$

整理得

$$u _ { j , k + 1 } = u _ { j , k } + \kappa \frac { \Delta \tau } { ( \Delta x ) ^ { 2 } } \left( u _ { j + 1 , k } - 2 u _ { j , k } + u _ { j - 1 , k } \right)$$

到这里得到一个迭代方程组：

$$U _ { j , k + 1 } = \rho U _ { j - 1 , k } + ( 1 - 2 \rho ) U _ { j , k } + \rho U _ { j + 1 , k } , \quad 1 \leqslant j \leqslant N - 1 , 0 \leqslant k \leqslant M - 1$$

其中 $\rho = \frac { \kappa \Delta \tau } { ( \Delta x ) ^ { 2 } }$ 。

下面使用 Python 代码实现微分到差分的格式替换过程。

- 首先定义基本变量：
    - N 为空间方向的网格数。
    - M 为时间方向的网格数。
    - T 为最大时间期限。
    - X 为最大空间范围。
    - U 为用来存储差分网格点上的值的矩阵。

```python
N = 25              # x 方向网格数
M = 2500            # t 方向网格数
T = 1.0
X = 1.0
xArray = np.linspace(0, X, N+1)
yArray = map(initialCondition, xArray)
starValues = yArray
U = np.zeros((N+1, M+1))
U[:, 0] = starValues
```

