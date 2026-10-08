假设 $S_{\max}$为股票价格所能达到的最大值，定义价格步长为 $\Delta S = S_{\max}/M$, 其中 $M$ 为价格步数。这样就有 $M+1$ 个股票价格点：

$$0, \Delta S, 2\Delta S, \cdots, S_{\max}$$

上面的价格点与时间点构成了一个共有 $(M+1)\times (N+1)$ 个坐标点的方格。任意点 $(i, j)$ 对应的时间是 $i\Delta t$, 股票价格是 $j\Delta S$。

用 $C_{i,j}$ 表示点 $(i, j)$ 的期权价格，这样就可以用离散算子逼近 $\frac{\partial f}{\partial t}, \frac{\partial f}{\partial S}, \frac{\partial^2 f}{\partial S^2}$ 各项，从而把上述偏微分方程转化为离散方程。

![](bbox=[0.66, 0.17, 0.89, 0.32])

偏微分方程的隐式有限差分法如图 16-1 所示。

通过对式(16-1)进行差分处理，得出隐式有限差分法的表达式：

$$a_j C_{i,j-1} + b_j C_{i,j} + c_j C_{i,j+1} = C_{i+1,j} \qquad \qquad (16-2)$$

式中：

$$a_j = \frac{1}{2} r j \Delta t - \frac{1}{2} \sigma^2 j^2 \Delta t$$

$$b_j = 1 + r \Delta t + \sigma^2 j^2 \Delta t$$

$$c_j = -\frac{1}{2} r j \Delta t - \frac{1}{2} \sigma^2 j^2 \Delta t$$

以上即为差分方程组，式(16-2)的解很多。要求得某些特定的解，需要给出边界条件，即左右边界条件，这里使用 Dirichlet 边界条件。

# ## 16.3 Python 应用实现

首先导入 SciPy 库：

```
```
import scipy as sp
from scipy.linalg import solve_banded
```
```

描述 Black-Scholes-Merton 模型方程结构的类 BSMModel 定义如下：

```
```
class BSMModel:
    def __init__(self, s0, r, sigma):
        self.s0 = s0
        self.x0 = math.log(s0)
        self.r = r
        self.sigma = sigma
    def log_expectation(self, T):
        return self.x0 + (self.r - 0.5 * self.sigma * self.sigma) * T
    def expectation(self, T):
        return math.exp(self.log_expectation(T))
    def x_max(self, T):
        return self.log_expectation(T) + 4.0 * self.sigma * math.sqrt(T)
    def x_min(self, T):
        return self.log_expectation(T) - 4.0 * self.sigma * math.sqrt(T)
```
```

描述本章涉及的产品的类 CallOption 定义如下：