```
if i == tSteps:
    lattice[i-1][j-1] = 0.5 * discount * (call_payoff(lattice[i][j]) + call_
 payoff(lattice[i][j-1]))
else:
    lattice[i-1][j-1] = 0.5 * discount * (lattice[i][j] + lattice[i][j-1])
print u'二叉树价格: %.4f' % lattice[0][0]
print u'解析法价格: %.4f' % BSMPrice(1, strike, spot, r, d, sigma, ttm, rawOutput = True)[0]
二叉树价格：14.2663
解析法价格：14.1978
```

# 13.2 用面向对象的方法实现二叉树算法

前面展示了二叉树算法的基本结构，但是前面的具体实现有两个缺点：

+   1. 没有明确的接口，不便于用户使用该算法。
2. 没有完整的封装，十分不利于算法的扩展。

下面给出一个基于 Python 类的二叉树算法实现。实际上，通过上面的实验性探索，可以 发现整个程序可以拆成 3 个互相独立的功能模块：

+   1. 二叉树框架，即树的框架结构，包括节点数以及基本参数的保存。
2. 二叉树类型描述，即具体数算法的参数，例如上例中的 Jarrow-Rudd 树。
3. 偿付函数，即到期的偿付形式 (payoff function)。

# 13.2.1 二叉树框架

BinomialTree 类负责二叉树框架的构造，也是基本的二叉树算法的调用入口。它 有 3 个成员：

+   1. 构造函数 (`__init__`)。负责接收用户定义的具体参数，例如 spot 等；真正的二叉 树构造方法由私有方法 `_build_lattice` 以及传入参数 `treeTraits` 共同完成。
2. 树构造细节 (`_build_lattice`)。负责具体的树构造过程，这里需要依赖从 `treeTraits` 获取的参数，例如 up, down。
3. 树回溯 (roll_back)。从树的叶节点向根节点回溯的过程。最终根节点的值即为期 权的价值。它要求的参数是 pay_off 函数。

```
# 二叉树框架(可以通过传入不同的 treeTraits 类型设计不同的二叉树结构)
class BinomialTree:
    def __init__(self, spot, riskFree, dividend, tSteps, maturity, sigma, treeTraits):
        self.dt = maturity / tSteps
        self.spot = spot
        self.r = riskFree
        self.d = dividend
        self.tSteps = tSteps
        self.discount = math.exp(-self.r * self.dt)
        self.v = sigma
        self.up = treeTraits.up(self)
        self.down = treeTraits.down(self)
        self.upProbability = treeTraits.upProbability(self)
        self.downProbability = 1.0 - self.upProbability
        self._build_lattice()
```