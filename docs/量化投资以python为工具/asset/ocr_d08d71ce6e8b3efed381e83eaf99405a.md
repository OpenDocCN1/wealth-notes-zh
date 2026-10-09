# ### 24.2.2 移动平均模型

MA(q) 模型认为因变量序列 $x_{t}$ 与随机冲击项的当前值 $\varepsilon_{t}$ 及 $q$ 期滞后值 $\varepsilon_{t-1}$, $\varepsilon_{t-2}$, $\cdots$, $\varepsilon_{t-q}$ 有关，而且是随机冲击项的加权平均，因此被称作移动平均模型。一个 $q$ 阶移动平均模型 MA(q) 可以用数学表达为：

$x_{t} = \mu + \varepsilon_{t} + \theta_{1}\varepsilon_{t-1} + \theta_{2}\varepsilon_{t-2} + \cdots + \theta_{q}\varepsilon_{t-q}$

其中 $\{\varepsilon_{t}\}$ 是白噪声序列，满足：

$\mathbb{E}\left(\varepsilon_{t}\right) = 0; \quad \text{Var}\left(\varepsilon_{t}\right) = \sigma_{\varepsilon}^{2}; \quad \mathbb{E}\left(\varepsilon_{t}\varepsilon_{s}\right) = 0, \forall s \neq t$

由于 MA(q) 仅仅是白噪声过程的线性组合，因此有：

$\begin{aligned} \mathbb{E}\left(x_{t}\right) & = \mu, \\ \text{Var}\left(x_{t}\right) & = \gamma_{0} = \left(1 + \theta_{1}^{2} + \theta_{2}^{2} + \cdots + \theta_{q}^{2}\right)\sigma_{\varepsilon}^{2}, \\ \rho_{l} & = \begin{cases} 1, & l=0 \\ \dfrac{\left(\theta_{l} + \theta_{l+1}\theta_{1} + \theta_{l+2}\theta_{2} + \cdots + \theta_{q}\theta_{q-l}\right)}{\left(1 + \theta_{1}^{2} + \theta_{2}^{2} + \cdots + \theta_{q}^{2}\right)}, & \forall l=1,2,\cdots,q \\ 0, & \forall l>q \end{cases} \end{aligned}$

由以上公式我们可以得知 MA(q) 模型一个很重要的统计性质：MA(q) 模型自相关系数 $q$ 阶截尾。所谓的 $q$ 阶截尾是指，在 $q$ 阶以后 MA(q) 模型的自相关系数马上截止，$q+1$ 阶起就等于 0（即上式 $\gamma_{l} = 0, \forall l > q$ 所表达的内容）。考虑 AR 模型和 MA 模型自相关系数的性质，我们可以根据自相关图，来初步判断所研究的时间序列大致符合什么类型的模型。

# ### 24.3 自回归移动平均模型

AR(p) 模型认为时间序列中 $x_{t}$ 的值与过去 $p$ 期的滞后值有关，MA(q) 模型则用滞后 $q$ 期的随机扰动项来解释当期的 $x_{t}$，不过在金融经济领域中，很多变量的值既会与自己过去期的表现有关系，又受到过去随机冲击的影响，ARMA 模型表达的就是这个思想。ARMA 模型是研究时间序列的重要方法，由 AR 模型与 MA 模型混合构成。ARMA(p,q) 模型的一般表达式为：

$x_{t} = \phi_{0} + \phi_{1}x_{t-1} + \phi_{2}x_{t-2} + \cdots + \phi_{p}x_{t-p} + \varepsilon_{t} + \theta_{1}\varepsilon_{t-1} + \theta_{2}\varepsilon_{t-2} + \cdots + \theta_{q}\varepsilon_{t-q}$

其中 $\{\varepsilon_{t}\}$ 是零均值白噪声序列，满足：