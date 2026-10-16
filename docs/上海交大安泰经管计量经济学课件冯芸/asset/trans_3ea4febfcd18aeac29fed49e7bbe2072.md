

## 多重共线性导致的后果

- **OLS**参数估计量的方差很大，估计精度差（同样的置信水平下，置信区间很宽）。
- **T**值很小，但**F**值很大

因变量：SR
方法：最小二乘法
日期：03/29/10  时间：17:49
样本：1 10
包含观测数：10

R²=0.997926
方差膨胀因子
VIF=1/(1-0.997926)=482.16>10

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | -0.386271 | 2.897956 | -0.133291 | 0.8973 |
| CF | 0.097923 | 0.001578 | 62.04047 | 0.0000 |

| Statistic | Value | Statistic | Value |
|---|---|---|---|
| R-squared | 0.997926 | Mean dependent var | 170.0000 |
| Adjusted R-squared | 0.997667 | S.D. dependent var | 60.55301 |
| S.E. of regression | 2.925035 | Akaike info criterion | 5.161346 |
| Sum squared resid | 68.44662 | Schwarz criterion | 5.221863 |
| Log likelihood | -23.80673 | Hannan-Quinn criter. | 5.094959 |
| F-statistic | 3849.020 | Durbin-Watson stat | 2.008509 |
| Prob(F-statistic) | 0.000000 |  |  |

- 如何消除不利影响？
  - 从模型中删除不重要的解释变量，但可能会引起模型设定偏误
  - 尽可能获取更多的样本数据
  - 重新考虑模型（或选择变量）

- 有效性或最佳性或最小方差性（所有线性、无偏估计量中方差最小）

## OLS估计值的抽样分布

- MLR.6 随机误差项 *u* 服从均值为零，（同）方差为 *σ²* 的正态分布

$$u_i \mid X \sim N(0, \sigma^2)$$

$$\hat{\beta}_j \mid X \sim N\left(\beta_j, Var\left(\hat{\beta}_j \mid X\right)\right)$$

$$\sim N\left(\beta_j, \dfrac{\sigma^2}{\sum \hat{r}_{ji}^2}\right)$$

f(y|x)

y

normal distributions

E(y|X) = β₀ + β₁X

X

X₁

X₂

X₃

## 经典线性模型假定

- **Classical Linear Model Assumption**

  **(CLM假定)：** 假定1至假定6

## 随机误差项方差的估计量

- 最小二乘估计量 $\hat{\sigma}^2 = \dfrac{\sum \hat{u}_i^2}{n - k - 1}$ 是 $\sigma^2$ 的无偏估计量
- 回归标准误（Standard Error of Regression）$\sqrt{\dfrac{\sum \hat{u}_i^2}{n - k - 1}}$：真实观测值 $y$ 偏离估计的回归函数的样本标准差

## 大样本性质
（asymptotic/large sample properties）

- 一致性是对估计量的基本要求
- 在假设1-假设4成立的前提下，OLS参数估计值是一致估计量。

plim[β̂_j (n)] = β_j

plim[β̂_j (n)] - β_j = Cov(x,u) / Var(x)

## 渐近正态性

$$\hat{\beta}_j=\frac{\sum \hat r_{ji}y_i}{\sum \hat r_{ji}^{2}}$$，在 MLR1~MLR5 假定下，应用中心极限定理，

- $\sqrt{n}(\hat{\beta}_j-\beta_j)\xrightarrow{a}N(0,\sigma^2/a_j^2)$，其中 $a_j^2=\operatorname{plim}\left(\frac{1}{n}\sum_{i=1}^{n}\hat r_{ji}^{2}\right)$
- $\operatorname{plim}\hat{\sigma}^{2}=\sigma^{2}$
- 对于 $j=1,\cdots,k$，$\frac{\hat{\beta}_j-\beta_j}{\operatorname{se}(\hat{\beta}_j)}=\frac{\hat{\beta}_j-\beta_j}{\sqrt{\hat{\sigma}^{2}/\sum_{i=1}^{n}\hat r_{ji}^{2}}}\xrightarrow{a}N(0,1)$，$t(n-k-1)\xrightarrow{a}N(0,1)$

## 六、回归参数的显著性检验和区间估计

- 回归参数的 t 检验（显著性检验）：检验每个解释变量对被解释变量边际影响的显著性。
- 回归参数的区间估计（置信区间）：显著性检验的另一种表述

### （一）回归参数的显著性检验：单个参数

H₀: βⱼ = 0 ⇔ 参数不显著(异于零)

H₁: βⱼ ≠ 0 ⇔ 参数显著(异于零)

### 问题

$$\hat{\beta}_j\mid X\sim N\left(\beta_j,\frac{\sigma^2}{\sum \hat{r}_{ji}^2}\right)$$

构造统计量，$$\frac{\hat{\beta}_j-\beta_j}{\sqrt{\sigma^2/\sum \hat{r}_{ji}^2}}\sim N(0,1)$$

### 问题

$$\frac{\hat{\beta}_j - \beta_j}{\sqrt{\sigma^2 / \sum \hat{r}_{ji}^2}} \sim N(0,1)$$

$$\frac{\hat{\beta}_j - \beta_j}{\sqrt{\hat{\sigma}^2 / \sum \hat{r}_{ji}^2}} \ ? \ N(0,1)$$

其中，$\hat{\sigma}^2 = \frac{\sum \hat{u}_i^2}{n - k - 1}$

### 答案：不成立，需要重新构造统计量

$$\frac{\hat{\beta}_j-\beta_j}{se(\hat{\beta}_j)}=\frac{\hat{\beta}_j-\beta_j}{\sqrt{\hat{\sigma}^2/\sum \hat{r}_{ji}^2}}\sim t(n-k-1)$$

其中，$k+1$为待估参数个数

![](img/27e6f65397bb7fd3eced98a716a58f50_267_0.png)

H₀ : βⱼ = 0 ⇔ 参数不显著(异于零)

H₁ : βⱼ ≠ 0 ⇔ 参数显著(异于零)

构造统计量 t = (β̂ⱼ - βⱼ) / √(σ̂² / Σ r̂ⱼi²) = (β̂ⱼ - βⱼ) / √V̂(β̂ⱼ) ~ t(n - k - 1)

进行双侧检验，即判断 |t| 是否大于 t_{α/2}

### $t$ 检验的决策规则

| 检验类型 | $H_0$：原假设 | $H_1$：备择假设 | 拒绝原假设 |
|---|---|---|---|
| 双侧检验 | $\beta=b$ | $\beta\ne b$ | $|t|>t_{\alpha/2,df}$ |
| 右侧检验 | $\beta=b$ | $\beta>b$ | $t>t_{\alpha,df}$ |
| 左侧检验 | $\beta=b$ | $\beta<b$ | $t<t_{\alpha,df}$ |

### 5% rejection rule for the alternative H₁: βⱼ ≠ 0 with 25 df.

![](img/27e6f65397bb7fd3eced98a716a58f50_270_0.png)

Area = .025

Area = .025

−2.06

0

2.06

rejection region

rejection region

因变量：SALARY
方法：最小二乘法
日期：03/15/10  时间：16:25
样本：1 209
包含观测数：209

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 963.1913 | 213.2403 | 4.516930 | 0.0000 |
| ROE | 18.50119 | 11.12325 | 1.663290 | 0.0978 |

| Statistic | Value | Statistic | Value |
|---|---|---|---|
| R-squared | 0.013189 | Mean dependent var | 1281.120 |
| Adjusted R-squared | 0.008421 | S.D. dependent var | 1372.345 |
| S.E. of regression | 1366.555 | Akaike info criterion | 17.28750 |
| Sum squared resid | 3.87E+08 | Schwarz criterion | 17.31948 |
| Log likelihood | -1804.543 | Hannan-Quinn criter. | 17.30043 |
| F-statistic | 2.766532 | Durbin-Watson stat | 2.104990 |
| Prob(F-statistic) | 0.097768 |  |  |

