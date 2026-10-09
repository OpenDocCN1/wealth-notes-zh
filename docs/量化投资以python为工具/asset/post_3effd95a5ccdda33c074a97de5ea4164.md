
$$\begin{aligned}\mathbb{E}\left(\varepsilon_{t}\right)&=\mathbb{E}\left[\mathbb{E}\left(\varepsilon_{t}\mid\mathcal{F}^{t-1}\right)\right]=0\\\operatorname{Var}\left(\varepsilon_{t}\right)&=\mathbb{E}\left[\mathbb{E}\left(\varepsilon_{t}^{2}\mid\mathcal{F}^{t-1}\right)\right]=\mathbb{E}\left(\sigma_{t}^{2}\right)\\&=\alpha_{0}+\alpha_{1}\mathbb{E}\left(\varepsilon_{t-1}^{2}\right)+\cdots+\alpha_{p}\mathbb{E}\left(\varepsilon_{t-p}^{2}\right)\\&=\frac{\alpha_{0}}{1-\alpha_{1}-\cdots-\alpha_{p}}\\\operatorname{Cov}\left(\varepsilon_{t}\varepsilon_{t-j}\right)&=\mathbb{E}\left(\varepsilon_{t}\varepsilon_{t-j}\right)=\mathbb{E}\left[\sigma_{t}\sigma_{t-j}u_{t}u_{t-j}\mathbb{E}\left(u_{t}\mid\mathcal{F}^{t-1}\right)\right]=0\end{aligned}$$

而且 $\varepsilon_{t}^{2}$ 有自相关性、并可用 AR(p) 模型刻画， $\varepsilon_{t}$ 满足厚尾现象，读者有兴趣可以自行验证。综合来看，ARCH(p) 模型很好地刻画了资产收益率序列表现出来的特征，不过需要注意的是，ARCH（或 GARCH）模型是完全单纯的统计模型，它们只是捕捉到金融序列的特征，而不能用来解释金融序列为何有这些特征。

ARCH(p) 模型的估计过程如下。

- 估计一个 ARCH 模型，首先需要设定一个合适的阶数 $p$。为确立 ARCH(p) 模型中的阶数，在检验得知时间序列 $\left\{\varepsilon_{t}\right\}$ 确实存在显著的 ARCH 效应的条件下，我们可以用 $\varepsilon_{t}^{2}$ 的偏自相关函数（PACF）来确定 $p$。
- 设好阶数以后，为估计模型参数，我们常常假定 $u_{t}$ 服从以下三种分布中的一种来进行估计：标准正态分布、标准化的学生 $t$ 分布和广义误差分布。然后用最大似然法 (Maximum Likelihood) ${ }^{1}$ 对模型参数进行估计。
- 对模型进行参数估计以后，需要检验模型设定。最常见的检验依赖于以下事实：一个正确设定的 ARCH 模型，标准化残差 $\hat{u}_{t}=\frac{\hat{\varepsilon}_{t}}{\hat{\sigma}_{t}}$ 是独立同分布的随机过程。因此 $\left\{\hat{u}_{t}\right\}$ 和 $\left\{\hat{u}_{t}^{2}\right\}$ 的 Ljung-Box 统计量可以分别用来检验均值方程和波动率方程的正确性。

### 25.2.2 GARCH 模型

从前面介绍可以看到，ARCH模型中波动率 $\sigma_{t}^{2}$ 仅与 $\varepsilon_{t}^{2}$ 的滞后项有关，即 ARCH(p) 模型的实质是，用残差平方序列的 $p$ 阶移动平均拟合当期条件方差函数值，也就是说 ARCH(p) 模型是将 AR(p) 模型的思想用在估计条件方差。Bollerslev (1986) 提出了 ARCH 模型的推广形式，称为广义自回归条件异方差模型（Generalized AutoRegressive Conditional Heteroskedastic Model），简称 GARCH 模型。GARCH(p,q) 类似于 ARMA(p,q) 的设定，认为时间序列每个时间点变量的波动率是最近 $p$ 个时间点残差平方的线性组合，与最近 $q$ 个时间点变量波动率的线性组合加起来得到：

$$\begin{aligned}\varepsilon_{t}&=\sigma_{t}u_{t}\\\sigma_{t}^{2}&=\alpha_{0}+\sum_{i=1}^{p}\alpha_{i}\varepsilon_{t-i}^{2}+\sum_{j=1}^{q}\beta_{j}\sigma_{t-j}^{2}.\end{aligned}$$

其中，$\{u_t\}$ 是一个均值为 0、方差为 1 的独立同分布随机变量序列，即 $u_t \sim IID(0, 1)$；$\alpha_0 > 0$，$\alpha_i, \beta_j \geqslant 0$，$\sum_{i=1}^{p} \alpha_i + \sum_{j=1}^{q} \beta_j < 1$，且假定 $\alpha_i$ 与 $\beta_j$ 满足一定条件使得 $\varepsilon_t$ 的条件方差随时间变化、无条件方差有限。

令 $\eta_t = \varepsilon_t^2 - \sigma_t^2 = \sigma_t^2 (u_t^2 - 1)$，在 ARCH 模型中我们已知：

$$ \text{Var}\left(\varepsilon_t\right) = \text{E}\left(\varepsilon_t^2\right) = \text{E}\left(\sigma_t^2\right) $$

则：

$$ \text{E}\left(\eta_t\right) = 0 $$

且对于 $j \geqslant 1$，

$$ \text{Cov}\left(\eta_t, \eta_{t-j}\right) = \text{E}\left(\eta_t \eta_{t-j}\right) = \text{E}\left[\sigma_t^2 \sigma_{t-j}^2 \left(u_t^2 - 1\right)\left(u_{t-j}^2 - 1\right)\right] = 0 $$

也就是说 $\eta_t$ 是一个白噪声过程。将 $\eta_t = \varepsilon_t^2 - \sigma_t^2$ 改为 $\sigma_t^2 = \varepsilon_t^2 - \eta_t$ 并代入 $\varepsilon_t^2$ 中，规定当 $i > p$ 时 $\alpha_i = 0$，且当 $j > s$ 时 $\beta_j = 0$，可得：

$$\begin{align*} \varepsilon_t^2 &= \sigma_t^2 + \varepsilon_t^2 - \sigma_t^2 \\ &= \alpha_0 + \sum_{i=1}^{p} \alpha_i \varepsilon_{t-i}^2 + \sum_{j=1}^{q} \beta_j \left(\varepsilon_{t-j}^2 - \eta_{t-j}\right) + \eta_t \\ &= \alpha_0 + \left(\sum_{i=1}^{p} \alpha_i \varepsilon_{t-i}^2 + \sum_{j=1}^{q} \beta_j \varepsilon_{t-j}^2\right) + \eta_t - \sum_{j=1}^{q} \beta_j \eta_{t-j} \\ &= \alpha_0 + \sum_{i=1}^{\max(p,q)} \left(\alpha_i + \beta_i\right) \varepsilon_{t-i}^2 + \eta_t - \sum_{j=1}^{q} \beta_j \eta_{t-j} \end{align*} $$

满足 ARCH(p) 模型的序列 $\{\varepsilon_t\}$ 的变化序列 $\{\varepsilon_t^2\}$ 可用 AR(p) 模型刻画，而 GARCH 模型相当于在变换后的 $\varepsilon_t^2$ 方程中添加了 MA 项（即 $\eta_t, \eta_{t-1}, \dots, \eta_{t-j}$ 项），以作为 ARCH 模型的推广。通过计算也可以得到 GARCH 模型中波动率聚集以及厚尾特性，在此略过细节，有兴趣的读者可自行推导。GARCH 模型的估计与 ARCH 模型类似，在假设扰动的分布之后常采用条件最大似然估计，具体过程请参阅计量经济学相关书籍。

ARCH 与 GARCH 模型的不足之处很类似。首先，这两种模型都认为波动是对称的，也就是不论何时，过去期的波动对现期间条件方差的影响是相同的，而实证结果并非如此，比如有学者发现，当坏（好）消息发布时，股票收益率的波动会增加（减小）。其次，两个模型对参数的限制都较强，特别是高阶模型，参数需要满足的约束会非常复杂。最后，作为统计模型，两个模型并没有提供关于波动率变化的更进一步的解释，而仅仅是拟合波动率变化的统计行为，要进一步理解波动率的动态变化还需要其他理论。

## 25.3 ARCH 效应检验

对资产收益率序列 $\{y_t\}$ 建立波动率模型一般需要下列 5 个步骤。

- 1. 先检验收益率序列 $\{y_t\}$ 是否是平稳的时间序列，并对根据其相关性建立合适的均值方程（比如 ARMA 模型），描述收益率的 $y_t$ 如何随时间演进。根据拟合的模型和实际值，可以得到残差序列 $\{\hat{\epsilon}_t\}$。
- 2. 对拟合的均值方程得到的残差序列 $\{\hat{\epsilon}_t\}$ 进行 ARCH 效应检验，也就是检验收益率围绕着均值的偏差是否看起来时大时小，而非非几乎同样规模的偏差。检验序列 $\{\hat{\epsilon}_t\}$ 是否具有 ARCH 效应有两种常用方法。
  - (a) 用 Ljung-Box 检验残差平方序列 $\{\hat{\epsilon}_t^2\}$ 的自相关性，即将序列的平方值作为波动率的代理变量，若残差序列有自相关性，则说明当期波动与过去期波动有关，初步判别序列 $\{\hat{\epsilon}_t\}$ 有 ARCH 效应。
  - (b) 用 Engle (1982) 提出的 Lagrange Multiplier 检验（LM 检验）。LM 检验的公式为：
    $$\hat{\epsilon}_t^2 = \hat{\alpha}_0 + \sum_{i=1}^p \hat{\alpha}_i \hat{\epsilon}_{t-i}^2 \quad (25.2)$$
    检验的原假设为：序列 $\{\hat{\epsilon}_t\}$ 无 ARCH 效应，即 $\alpha_i = 0, i = 1, \dots, q$。备择假设为序列 $\{\hat{\epsilon}_t\}$ 具有 ARCH 效应，即至少有一个 $\alpha_i$ 显著不为 0。
- 3. 若第 2 步中 ARCH 效应在统计上显著，也就是收益率关于其均值的偏差也有动态变化，那么我们就需要再设定一个波动率模型试图刻画这种波动率的动态变化；
- 4. 对均值方程和波动率方程进行联合估计，也就是假设实际数据服从前面设定的均值方程和波动率方程后，计算出均值方程和波动率方程中的参数的估计值是多少，以及参数估计的误差是多少；
- 5. 检验所拟合的模型，并在必要时进行改进。这一步非常重要，因为第 4 步中假定实际数据服从第 1 步设定的均值方程和第 3 步设定的波动率方程，然而这一假定并不一定合理。如果估计结果（特别是残差）不满足模型本身的假设，那么模型的可用性就很值得怀疑。所以我们需要在估计完参数后对模型的假设再进行检验。

下面，我们以上证指数为例来讲解如何判断是否具有 ARCH 效应。我们以上证指数 2009 年 1 月到 2013 年 4 月的日收益率序列的平方以及绝对值这两个序列的图形上来识别原序列是否具有 ARCH 效应。

```python
In [1]: import pandas as pd

# 读取上证综指数序列文件
In [2]: SHret=pd.read_table('025/TrID_IndexSum.txt',index_col='TrIdt',sep='\t')

In [3]: SHret.index=pd.to_datetime(SHret.index)

In [4]: SHret=SHret.sort()
```

```python
In [5]: import matplotlib.pyplot as plt
In [6]: import numpy as np
In [7]: plt.subplot(211)
    ...: plt.plot(SHret**2)
    ...: plt.xticks([])
    ...: plt.title('Squared Daily Return of SH Index')
    ...:
    ...: plt.subplot(212)
    ...: plt.plot(np.abs(SHret))
    ...: plt.title('Absolute Daily Return of SH Index')
Out[7]: <matplotlib.text.Text at 0x7fe5bdb4e4eb8>
```

如图 25.1 所示，可以大致观察出上证指数收益率序列存在着很明显的波动聚集的现象。因此我们可以初步判断出上证指数日收益率序列存在着 ARCH 效应。

![](img/0de14a685304f8aa267f6492145864ac_387_0.png)

接着我们用 LB 检验来检验收益率平方的自相关性（取 13 阶滞后）：

```python
In [8]: from statsmodels.tsa import stattools
In [9]: LjungBox=stattools.q_stat(stattools.acf(SHret**2)[1:13],len(returns))
In [10]: LjungBox[1][-1]
Out[10]: 1.6197853208184228e-33
```

由于检验的 p 值明显小于 0.05，所以可以拒绝上证指数收益率的平方序列是白噪声（无自相关）的原假设，即原序列（上证指数收益率序列）存在 ARCH 效应。

## 25.4 GARCH 模型构建

最常用且通常也很适合金融时间序列的 GARCH 模型是 GARCH(1,1)。在 Python 中，我们可以使用 arch 包来对 GARCH 模型进行建模。接下来，我们就以 2009—2015 年上证指数的收益率为研究对象，建立 GARCH 模型。具体代码如下所示：

```python
#导入arch包中的arch_model模块
In [11]: from arch import arch_model

#设定模型
#arch_model默认建立GARCH(1,1)模型
In [12]: am = arch_model(SHret)

#估计参数
#update_freq=0表示不输出中间结果，只输出最终结果
In [13]: model = am.fit(update_freq=0)
Positive directional derivative for linesearch (Exit mode 8)
Current function value: -4464.73582193
Iterations: 9
Function evaluations: 61
Gradient evaluations: 5
```

```python
#查看结果
-In [14]: print(model.summary())
Constant Mean - GARCH Model Results
==============================================================================
Dep. Variable:          Retindex   R-squared:              -0.000
Mean Model:             Constant   Adj. R-squared:          -0.000
Vol Model:              GARCH       Log-Likelihood:         4464.74
Distribution:           Normal      AIC:                   -8921.47
Method:                 Maximum     BIC:                   -8900.16
                       Likelihood  No. Observations:        1522
Date:                May 11 2016   Df Residuals:            1518
Time:               21:56:56      Df Model:                    4
                              Mean Model
==============================================================================
             coef   std err       t     P>|t|     95.0% Conf. Int.
------------------------------------------------------------------------------
mu         3.3362e-04 1.012e-07  3296.428   0.000  [3.334e-04,3.338e-04]
             Volatility Model
==============================================================================
            coef    std err        t      P>|t|    95.0% Conf. Int.
--------------------------------------------------------------------------------------
omega     3.7169e-06 6.660e-27   5.581e+20     0.000   [3.717e-06,3.717e-06]
alpha[1]    0.0500   1.120e-07   4.464e+05      0.000   [5.000e-02,5.000e-02]
beta[1]      0.9300   1.025e-05    9.070e+04     0.000   [0.930,    0.930]
==============================================================================

Covariance estimator: robust
```

根据函数的返回值，模型结果为：

> $r_t = -0.00033362 + \varepsilon_t$
