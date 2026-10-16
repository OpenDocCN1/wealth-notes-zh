

## 样本均值的抽样分布或概率分布

- **样本均值（总体均值的估计量）的方差（抽样方差）** 是 $\sigma^2/n$。当 $n\to\infty$ 时，样本均值的抽样方差 $\to 0$。

## 样本均值的抽样分布或概率分布

若 $y_1，y_2，……，y_n$ 是来自于均值为 $\mu$，方差为 $\sigma^2$ 的正态总体的一组随机样本，则样本均值（总体均值的估计量）也服从正态分布。

$$\bar{y}\sim N\left(\mu,\frac{\sigma^2}{n}\right)$$

- 另一个 μ 的线性无偏估计量是 y₁

$$E(y_1)=\mu$$

- y₁ 的抽样方差是 σ²

![](img/27e6f65397bb7fd3eced98a716a58f50_233_0.png)

- 有时有偏估计量方差更小。兼顾无偏性和小方差的指标是估计量的均方误(Mean Squared Error, MSE)

MSE(β̂) = E(β̂ − β)² = Var(β̂) + [E(β̂) − β]²

## 一致性：

记 β̂(n) 是基于样本容量为 n 的一组样本 y₁,…,yₙ 得到的参数估计值，可以看作是 n 的函数。随着 n → ∞，对于任意小的正数 ε，均有 limₙ→∞ P(|β̂(n) - β| ≥ ε) = 0，即 β̂(n) 的概率极限是 β，称 β̂(n) 是总体参数 β 的一致估计量，记作 plim[β̂(n)] = β。

## 大数定理

- 随机样本 $y_1, y_2, \dots y_n$，总体均值 $\mu$。

$\bar{y} = \frac{1}{n} \sum_{i=1}^{n} y_i$

$E(\bar{y}) = \mu$

$\mathrm{plim}(\bar{y}) = \mu$

![](img/27e6f65397bb7fd3eced98a716a58f50_237_0.png)

## 累积分布函数

![](img/27e6f65397bb7fd3eced98a716a58f50_239_0.png)

## 概率极限的一些性质

假设 $\operatorname{plim}[\hat{\alpha}(n)] = \alpha$，$\operatorname{plim}[\hat{\beta}(n)] = \beta$，$Z_n \sim N(0,1)$，$Z \sim N(0,1)$

- 1. 函数的概率极限等于概率极限的函数
- 连续函数 $g(\cdot)$，有 $\operatorname{plim}\{g[\hat{\beta}(n)]\}=g\{\operatorname{plim}[\hat{\beta}(n)]\}$

- 2. 和的概率极限等于概率极限的和
- $\operatorname{plim}[\hat{\alpha}(n)+\hat{\beta}(n)]=\alpha+\beta$

- 3. 乘积的概率极限等于概率极限的乘积
- $\operatorname{plim}[\hat{\alpha}(n)\cdot\hat{\beta}(n)]=\alpha\cdot\beta$

- 4. 除的概率极限等于概率极限相除
- $\operatorname{plim}\left[\frac{\hat{\alpha}(n)}{\hat{\beta}(n)}\right]=\frac{\alpha}{\beta}$

- 5. $\hat{\alpha}(n)Z(n)\xrightarrow{a}N(0,\alpha^2)$

![](img/27e6f65397bb7fd3eced98a716a58f50_240_0.png)

## 五、OLS估计量的统计性质

- OLS估计方法得到的样本回归线是否是总体回归线的一个好的估计量？
- 回答上述问题，需要了解参数估计量的抽样分析，因此需要对随机误差项等进行一系列的假定。

![](img/27e6f65397bb7fd3eced98a716a58f50_242_0.png)

## 横截面回归的经典假定
（Gauss-Markov假定）

- **MLR.1** 模型对参数而言是线性的。
  $y_i = \beta_0 + \beta_1 x_{1i} + \beta_2 x_{2i} + \cdots + \beta_k x_{ki} + u_i$
  $i = 1, \cdots, n$

- **MLR.2** $n$次观测 $\{(y_i,\ x_{i1},\ \cdots,\ x_{ik}), i=1,\cdots,n\}$ 为随机样本（第 $i$ 次观测和第 $j$ 次观测来自于独立同分布的总体）

## 横截面回归的经典假定
（Gauss-Markov假定）

- MLR.3 扰动项的条件期望为零 $E(u|x_1, ..., x_k)=0$（$E(u)=0$, $COV(u, x_j)=0$），称为外生解释变量

- MLR.4 解释变量之间不存在完全的线性相关关系（模型的可识别条件）

## OLS的统计性质：一阶矩

- 线性：参数估计量 $\hat{\beta}_j$ 是 $y_i$ 的线性函数

$$\hat{\beta}_j = \frac{\sum \hat{r}_{ji} y_i}{\sum \hat{r}_{ji}^2}$$

- 无偏性

$$E(\hat{\beta}_j \mid \mathbf{X}) = \beta_j, \quad j = 0, \cdots, k$$

$$E(\hat{\beta}_j) = \beta_j, \quad j = 0, \cdots, k$$

## OLS的统计性质：二阶矩

- **MLR.5** 同方差性

$$Var(u\mid X_1,X_2,\cdots,X_k)=\sigma^2$$

$$Var(\hat{\beta}_j\mid X)=\frac{\sigma^2}{\sum_i\hat r_{ji}^2}=\frac{\sigma^2}{(1-R_j^2)SST_j}$$

## OLS估计量的抽样方差（估计精度）

- 估计量的条件方差依赖于三个因素
  - σ²
  - SST_j 取决于解释变量样本数据的变动
  - R_j² 取决于解释变量之间的共线性强度（完全的共线性会出现什么情况？）

Var(\hat{β}_j | X) = \frac{σ²}{\sum \hat{r}_{ji}²} = \frac{σ²}{(1 - R_j²) SST_j}

![](img/27e6f65397bb7fd3eced98a716a58f50_249_0.png)

\[\operatorname{Var}(\hat{\beta}_1\mid X)=\frac{\sigma^2}{(1-R_1^2)SST_1}\]

\[\frac{\sigma^2}{SST_1}\]

横轴：\(R_1^2\)，范围为 0 到 1。

## 多重共线性程度的判断

- 方差膨胀因子VIF

$VIF = \frac{1}{(1 - R_1^2)}$

$Var(\hat{\beta}_1 | X) = \frac{\sigma^2}{SST_1} VIF$

- 如果一个变量的VIF超过10，就认为该变量是高度共线性的。

Dependent Variable: XF
Method: Least Squares
Date: 03/29/10  Time: 17:44
Sample: 1 10
Included observations: 10

## 例子：消费支出与收入和财富的关系

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 24.77473 | 6.752500 | 3.668972 | 0.0080 |
| SR | 0.941537 | 0.822898 | 1.144172 | 0.2902 |
| CF | -0.042435 | 0.080664 | -0.526062 | 0.6151 |

R-squared 0.963504  Mean dependent var 111.0000
Adjusted R-squared 0.953077  S.D. dependent var 31.42893
S.E. of regression 6.808041  Akaike info criterion 6.917411
Sum squared resid 324.4459  Schwarz criterion 7.008186
Log likelihood -31.58705  Hannan-Quinn criter. 6.817830
F-statistic 92.40196  Durbin-Watson stat 2.890614
Prob(F-statistic) 0.0000009

