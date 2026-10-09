
```python
In [26]: test_return = pd.DataFrame(test_return, index=test_set.index)
```

```python
In [27]: test_cum_return = (1 + test_return).cumprod()
```

```python
# 按照已生成的组合进行
In [28]: sim_weight = np.random.uniform(0, 1, (100, 5))
```

```python
In [29]: sim_weight = np.apply_along_axis(lambda x: x/sum(x), 1, sim_weight)
```

```python
In [30]: sim_return = np.dot(test_set, sim_weight.swapaxes(0, 1))
```

```python
In [31]: sim_return = pd.DataFrame(sim_return, index=test_cum_return.index)
```

```python
In [32]: sim_cum_return = (1 + sim_return).cumprod()
```

```python
In [33]: plt.plot(sim_cum_return.index, sim_cum_return, color='green')
....: plt.plot(test_cum_return.index, test_cum_return)
Out[33]: [matplotlib.lines.Line2D at 0x7fd188dico5o2]
```

![](img/0de14a685304f8aa267f6492145864ac_307_0.png)

图 19.7 最优配置与随机配置的绩效比较

## 19.4 Black-Litterman 模型

前文我们提到投资人做资产配置的前提是对未来形成预期（也就是已知未来各资产 $R_{1}, R_{2}, \cdots, R_{N}$ 的概率分布），然后利用 Markowitz 模型求得最优资产配比。Markowitz 模型输入参数包括历史数据法和情景分析法两种方法。情景分析法的缺点是主观因素、随意性太强，因此在实务上，绝大多数人用各资产的历史收益率资料来替代未来的预期收益率资料。整个资产配置过程基本上与 Python 实现的流程类似：先获取各资产的历史收益率，根据历史资料得到各资产的收益率均值和方差协方差矩阵，作为 Markowitz 模型的输入变量，最后得出最优资产配置比。整个过程仔细琢磨起来显然有不足之处：基于历史资料进行计算的基础是，我们相信未来各资产的收益率分布与过去相同，但是在现实中，过去收益率高的股票可能由于突发事件面临破产，债券收益率可能会由于央行对利息的调控而发生变化。简言之，历史并不一定完全重演。

尤有甚者，历史数据法，对样本区间非常敏感。比如在 2003 年年底时估计中国股市的预期收益率，如果从 2002—2003 年采样，估计的预期收益率为 10.3%；如果从 2001—2003 年采样，则估计的预期收益率为 −4.63%，究竟何者具有代表性值得存疑；又如从 1998—2003 年采样，则估计的预期收益率为 5.3%；如果从 1990—2003 年采样，则估计的预期收益率为 23.2%。由此可见，历史数据得到的参数会因为样本区间不同而大不相同，如果参数不一样，那么模型的输出结果将有很大的差异。

1992 年，在高盛（Goldman Sachs）工作的 Fischer Black 和 Robert Litterman 对全球债券投资组合研究时发现，当对德国债券预期报酬率做 0.1% 小幅修正之后，该类资产的投资比例竟然由原来的 10.0% 提高至 55.0%。这个发现说明 Markowitz 模型得到的最优资产配置比对输入参数过于敏感。

从以上叙述可以看出，根据历史信息进行现在的资产配置，并期待未来的投资收益达到某个水平，这整个过程有改善之处。Black 和 Litterman 提出了改进的模型——Black-Litterman 模型（简称 BL 模型），其核心思想是将投资者对大类资产的观点（主观观点）与市场均衡收益（先验预期收益率）相结合，从而形成新的预期收益率（后验预期收益率）。如果投资者没有特别的观点，则依旧使用先验收益；如果投资者对某些资产有特别的观点，则根据主观观点的信心水平来调整后验收益，从而影响投资组合配置。Black-Litterman 模型自提出以来，逐渐被华尔街主流所接受，现已成为高盛公司资产管理部门在资产配置上的一个主要工具。

现在我们简要介绍一下 BL 模型的求解过程。现在有 N 种资产，其收益率为 $R=\{R_{1}, R_{2}, \cdots, R_{N}\}$，BL 模型中假设 R 服从联合正态分布，即 $R \sim N(\mu, \Sigma)$，其中 $\mu$ 和 $\Sigma$ 为各资产预期收益率的期望值和协方差的估计值。现在假设估计向量 $\mu$ 本身也是随机的，并且服从正态分布：

$$
\mu \sim N(\pi, \tau\Sigma)
$$

其中 $\pi$ 为先验预期收益率之期望值，$\tau$ 在后文会详细解释。BL 模型的特色是允许考虑投资者个人的观点（Investor Views），处理的方式是用各资产收益率之线性方程组来代表主观观点，比如一条观点可以用以下公式表达：

$$
p_1\mu_1+p_2\mu_2+\cdots+p_N\mu_N=q_i+\epsilon_i
$$

其中 $\epsilon_{i}$ 为这条观点的误差项，$\epsilon_{i} \sim N(0, \sigma_{i}^{2})$，$\sigma_{i}^{2}$ 受投资人对此条观点的信心水平（Confidence Level）影响。投资者表达总体观点可以用：

$$
P\mu \sim N(Q, \Omega)
$$

其中：

- $P$（被称为 "Pick Matrix"）为 $K \times N$ 矩阵，即对 N 个资产有 K 个观点；
- $q$ 为看法向量 ($K \times 1$ 列向量)；
- $\Omega$ 为看法向量 $q$ 的误差项的协方差矩阵，表示投资者的观点与真实情况有所差别，$\Omega$ 为 $K \times K$ 的对角矩阵。不同的文献对其构造方式不同，常见的方法有以下四种。
    - 与先验预期收益率之方差成正比，比如 He 和 Litterman (1999) 设定：

    $$
    \omega_{ij} = \begin{cases} p(\tau\Sigma)P^{T} & \forall i=j \\ 0 & \forall i \neq j \end{cases}
    $$

    或者

    $$
    \Omega = \operatorname{diag}(P(\tau\Sigma)P^{T})
    $$

    这种方法在很多文献中都很常见，这里就采用该方法来确定 $\Omega$；
    - 使用信心水平；
    - 利用因子模型的残差项的方差；
    - 用 Idzorek (2005) 的方法来确定信心水平；

比如现在市场有五种资产 A、B、C、D、E，投资人基于自己的经验及分析，形成以下判断：

- 资产 A 的收益率为 15%；
- 资产 B 比资产 D 的收益率高 5%；
- 资产 B 和 C 比资产 A 的收益率低 6%。

据此，可以写出反应投资人主观观点的矩阵：

$$
P = \begin{bmatrix} 1 & 0 & 0 & 0 & 0 \\ 0 & 1 & 0 & -1 & 0 \\ -1 & 1/2 & 1/2 & 0 & 0 \end{bmatrix}
$$

看法向量：

$$
q = [15\%, 5\%, 6\%]^{T}
$$

看法向量误差项的协方差矩阵：

$$
\Omega = \operatorname{diag}(P(\tau\Sigma)P^{T})
$$

根据贝叶斯法则，结合先验信息与投资者观点的各资产预期收益率的期望值 $\mu_{|q; \Omega}$ 服从正态分布：

$$
\mu_{|q; \Omega} \sim N(\mu_{BL}, \Sigma_{BL}^{\mu})
$$

其中期望值 $\mu_{BL}$ ($N \times 1$ 列向量) 和协方差矩阵 $\Sigma_{BL}^{\mu}$ 为：

$$
\mu_{BL} = \left[ (\tau \Sigma)^{-1} + P^{T} \Omega^{-1} P \right]^{-1} \left[ (\tau \Sigma)^{-1} \pi + P^{T} \Omega^{-1} q \right]
$$

$$
\Sigma_{BL}^{\mu} = \left[ (\tau \Sigma)^{-1} + P^{T} \Omega^{-1} P \right]^{-1}
$$

其中 $\tau$ 为比例系数（常数），反应主观观点相对于先验信息的比重。最后，我们可以得到结合先验信息与投资者观点的各资产后验预期收益率分布（Posterior distribution）为：

$$
R_{|q; \Omega} = \mu_{|q; \Omega} + Z
$$

其中 $Z$ 为误差项，$Z \sim N(0, \Sigma)$。因此最后各资产后验预期收益率的期望值和方差分别为：

$$
\mathbb{E}\left(R_{|q; \Omega}\right) = \mu_{BL}
$$

$$
\Sigma_{BL} = \Sigma + \Sigma_{BL}^{\mu}
$$

有了各资产之预期收益分布，特别是预期收益率之期望值与方差，就可以根据最小化风险，或者最大化收益率来求解各资产的配比状况。总的说来，BL 模型求解过程包括以下几步。

(1) 使用历史数据估计预期收益率的协方差矩阵，假设根据 N 种资产过去 T 期的收益率资料，则协方差矩阵为：

$$
\hat{\Sigma} = \begin{bmatrix} \hat{\sigma}_{11} & \hat{\sigma}_{12} & \cdots & \hat{\sigma}_{1N} \\ \hat{\sigma}_{21} & \hat{\sigma}_{22} & \cdots & \hat{\sigma}_{2N} \\ \vdots & \vdots & \ddots & \vdots \\ \hat{\sigma}_{N1} & \hat{\sigma}_{N2} & \cdots & \hat{\sigma}_{NN} \end{bmatrix}
$$

式中 $\hat{\sigma}_{ij}$ 是根据样本数据估计出来的资产 i 与 j 的协方差，表达式为 $\hat{\sigma}_{ij} = \mathbb{E}([(R_{i,t} - \hat{\mu}_{i})(R_{j,t} - \hat{\mu}_{j})])$，其中 $\hat{\mu}_{i}$ 为资产 i 的历史平均收益率，$\hat{\mu}_{i} = \frac{\sum_{t=1}^{T} R_{i,t}}{T}$。

(2) 确定市场预期之收益率向量（N × 1 列向量），也就是先验预期收益之期望值。我们可以用历史收益率均值作为先验预期收益之期望值，即 $\pi = \hat{\mu}$。也可以根据市场价格、市场组合同构成反推出市场隐含的均衡收益率（Implied Equilibrium Return Vector）来作为先验预期收益之期望：

$$
\pi = \lambda \hat{\Sigma} \omega^{m}
$$

其中 $\lambda=\frac{E\left[R_{m}\right]-R_{f}}{\sigma_{m}^{2}}$ 为市场组合单位风险的超额收益，反应了市场对风险趋避的程度（$R_{m}$ 为市场组合之收益率，$R_{f}$ 为无风险收益，$\sigma_{m}^{2}$ 为市场组合收益率之方差），$\omega^{m}$ 为市场组合各资产的投资比例，$\hat{\Sigma}$ 为估计出来的协方差矩阵。还可以根据 CAPM 模型中的 Alpha (历史超额收益率) 来估计 $\pi$。

(3) 融合投资人的个人观点，即确定 $P$、$q$、$\Omega$。

(4) 修正后验收益。

(5) 投资组合优化：通过 Black-Litterman 模型，我们可以获得后验收益的期望值与协方差矩阵。根据这些信息，我们可以进行资产配置，比如利用 Markowitz 模型进行资产配置。

### Black-Litterman 模型之 Python 实现

Python 并未向我们提供实现 Black-Litterman 模型的函数，但是，我们可以自己写函数来实现 Black-Litterman 模型。我们以五只股票（白云机场、华夏银行、浙能电力、福建高速和生益科技）作为投资标的物，根据 BL 模型对求解过程做以下设定。

- (1) 计算出历史收益率之均值 $\mu$ 及协方差矩阵 $\hat{\Sigma}$；
- (2) 用历史收益率均值作为先验预期收益之期望值，即 $\pi=\mu$；
- (3) 结合投资者个人的观点（由 $P$、$Q$、$\Omega$ 代表，其中 $P$、$Q$ 为已知的输入变量，$\Omega$ 要自己计算）；
- (4) 根据公式计算出后验分布之期望值、协方差。

现在我们自己定义函数 blacklitterman() 来计算后验分布之期望值、协方差，该函数的输入变量有股票的历史收益率（returns），反应主观观点相对于先验信息的比重（tau），以及代表投资人看法的 $P$ 和 $Q$。首先我们以历史收益率均值（mu）和协方差矩阵（sigma）作为先验预期收益之期望值和协方差，然后根据前文的公式计算后验分布之期望值（er）和协方差（posteriorSigma）。

```python
# 定义函数
In [34]: def blacklitterman(returns, tau, P, Q):
...:     mu = returns.mean()
...:     sigma = returns.cov()
...:     pi = mu
...:     ts = tau * sigma
...:     Omega = np.dot(np.dot(P, ts), P.T) * np.eye(Q.shape[0])
...:     middle = np.linalg.inv(np.dot(np.dot(P, ts), P.T) + Omega)
...:     er = np.expand_dims(pi, axis=0).T + np.dot(np.dot(ts, P.T), middle).dot(Q - np.expand_dims(np.dot(P, pi), axis=0).T)
...:     posteriorSigma = sigma + ts - np.dot(ts.dot(P.T).dot(middle).dot(P), ts)
...:     return [er, posteriorSigma]
```

我们延续 Markowitz 模型的 5 只股票资产分配的例子，在这里要加上投资者的个人观点进行分析。现在假设某股票分析师认为白云机场、华夏银行、浙能电力、生益科技四只股票的日均收益率将达到 0.3%；两只交通类股票白云机场和福建高速的日均收益将比浙能电力高 0.1%。以此为根据构建个人观点。

```python
# 构造投资人的个人观点
# 构建资产选择矩阵 P，假设分析师认为
# 1. 前 4 只股票收益率为日均 0.3%
# 2. 两只交通股日均收益比浙能电力高 0.1%
In [35]: pick1 = np.array([1, 0, 1, 1, 1])
In [36]: q1 = np.array([0.003*4])
In [37]: pick2 = np.array([0.5, 0.5, 0, 0, -1])
In [38]: q2 = np.array([0.001])
In [39]: P = np.array([pick1, pick2])
In [40]: Q = np.array([q1, q2])

In [41]: P
Out[41]:
array([[ 1. , 0. , 1. , 1. , 1. ],
       [ 0.5, 0.5, 0. , 0. , -1. ]])

In [42]: Q
Out[42]:
array([[ 0.012],
       [ 0.001]])

# 修正后收益率
In [43]: res = blacklitterman(sh_return, 0.1, P, Q)

In [44]: p_mean = pd.DataFrame(res[0], index=sh_return.columns, columns=['posterior_mean'])

In [45]: p_mean
Out[45]:
         posterior_mean
byjc         0.002857
fjgs         0.003931
hxyh         0.002629
sykj         0.002921
zndl         0.002337

In [46]: p_cov = res[1]

In [47]: p_cov
Out[47]:
        byjc      fjgs      hxyh     sykj     zndl
byjc  0.000353  0.000246  0.000134  0.000132  0.000180
fjgs  0.000246  0.000506  0.000138  0.000155  0.000201
hxyh  0.000134  0.000138  0.000515  0.000085  0.000179
sykj  0.000132  0.000155  0.000085  0.000385  0.000127
zndl  0.000180  0.000201  0.000179  0.000127  0.000494
```

得到了后验收益的期望值与协方差矩阵之后，就可以利用 Markowitz 模型进行资产的配置。这里我们定义新的函数 blminVar() 来求解资产配置权重。该函数的输入变量是上面自定义函数 blacklitterman() 的输出结果（blres），以及投资人的目标收益率（goalRet），其中 blres 包含的信息是资产的后验分布之均值和协方差。

```python
In [48]: def blminVar(blres, goalRet):
...:     coefs = np.array(blres[1])
...:     means = np.array(blres[0])
...:     L = np.append(np.append((coefs.swapaxes(0, 1), [means.flatten()], 0), [np.ones(len(means))], 0).swapaxes(0, 1)
...:     L2 = list(np.ones(len(means)))
...:     L2.extend([0, 0])
...:     L3 = list(means)
...:     L3.extend([0, 0])
...:     L4 = np.array([L2, L3])
...:     L = np.append(L, L4, 0)
...:     results = np.linalg.solve(L, np.append(np.zeros(len(means)), [1, goalRet], 0))
...:     return (pd.DataFrame(results[:-2], index=blres[1].columns, columns=['p_weight']))
```

将前面加入投资人观点的资产后验分布结果 res 及投资人目标收益率（年收益率为 75%）作为 blminVar() 函数的输入变量，可得最优资产配置比。

```python
In [49]: blminVar(res, 0.75/252)
Out[49]:
       p_weight
byjc    0.226360
fjgs    0.172357
hxyh    0.201557
sykj    0.321434
zndl    0.078292
```

## 习题

1. 已知有下表所示的 5 种不同资产组合的风险和收益率的信息。哪种资产组合不是有效的资产组合？

| 资产组合 | 预期收益率 | 风险 |
| :---: | :---: | :---: |
| A | 0.05 | 0 |
| B | 0.07 | 0.12 |
| C | 0.07 | 0.06 |
| D | 0.07 | 0.04 |
| E | 0.05 | 0.06 |
