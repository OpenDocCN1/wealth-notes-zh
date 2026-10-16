
## 多元对数线性回归模型

- 偏斜率系数又称为偏弹性系数：控制其它因素不变

$$\ln y_i = \beta_0 + \beta_1 \ln x_{1i} + \beta_2 \ln x_{2i} + u_i$$

![](img/27e6f65397bb7fd3eced98a716a58f50_344_0.png)

## 实例：柯布一道格拉斯生产函数

Dependent Variable: LOG(GDP)
Method: Least Squares
Date: 10/25/04   Time: 20:50
Sample: 1955 1974
Included observations: 20

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---:|---:|---:|---:|
| C | -1.652419 | 0.606198 | -2.725873 | 0.0144 |
| LOG(LABOUR) | 0.339732 | 0.185692 | 1.829548 | 0.0849 |
| LOG(CAPITAL) | 0.845997 | 0.093352 | 9.062488 | 0.0000 |

| Statistic | Value | Statistic | Value |
|---|---:|---|---:|
| R-squared | 0.995080 | Mean dependent var | 12.22605 |
| Adjusted R-squared | 0.994501 | S.D. dependent var | 0.381497 |
| S.E. of regression | 0.028289 | Akaike info criterion | -4.155221 |
| Sum squared resid | 0.013604 | Schwarz criterion | -4.005861 |
| Log likelihood | 44.55221 | F-statistic | 1719.231 |
| Durbin-Watson stat | 0.425667 | Prob(F-statistic) | 0.000000 |

- 产出对劳动投入的弹性：**0.3397**
- 产出对资本投入的弹性：**0.8460**
- 规模报酬参数：**β₁+β₂**
  - **β₁+β₂=1**：规模报酬不变
  - **β₁+β₂>1**：规模报酬递增
  - **β₁+β₂<1**：规模报酬递减

- scalar
  t=(eq01.@coefs(2)+eq01.@coefs(3)-1)/(eq01.@cov(2,2)+eq01.@cov(3,3)+2*eq01.@cov(2,3))^0.5
- scalar tthr=@qtdist(0.95,eq01.@regobs-eq01.@ncoef)

## 例子：度量增长率（对数—线性模型）

$r$：$y_i$的增长率（复利率）

$y_i=y_0(1+r)^t$ ⇒ 增长模型

![](img/27e6f65397bb7fd3eced98a716a58f50_349_0.png)

$\ln y_i=\ln y_0+t\cdot\ln(1+r)$

![](img/27e6f65397bb7fd3eced98a716a58f50_349_1.png)

$\ln y=\beta_0+\beta_1t$

$\beta_0=\ln y_0，\quad \beta_1=\ln(1+r)$

![](img/27e6f65397bb7fd3eced98a716a58f50_349_2.png)

y = β₀ + β₁t ⇒ 线性趋势模型

β₁：年绝对增长

β₁/β₀：相对于年初的相对增长率（单利率）

## 工资和教育

- 教育是否对劳动力的工资产生影响
- 工资以每小时美元数来度量，教育程度以受教育的年数来度量
- 以1976年的劳动力为总体，数据来自1976年人口调查数据（Current Population Survey），由MIT的Henry Farber和Wooldridge在1988收集得到。

## 该模型存在什么问题？

Dependent Variable: WAGE
Method: Least Squares
Date: 04/19/10   Time: 10:50
Sample: 1 526
Included observations: 526

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---:|---:|---:|---:|
| C | -0.904852 | 0.684968 | -1.321013 | 0.1871 |
| EDUC | 0.541359 | 0.053248 | 10.16675 | 0.0000 |

| Statistic | Value | Statistic | Value |
|---|---:|---|---:|
| R-squared | 0.164758 | Mean dependent var | 5.896103 |
| Adjusted R-squared | 0.163164 | S.D. dependent var | 3.693086 |
| S.E. of regression | 3.378390 | Akaike info criterion | 5.276470 |
| Sum squared resid | 5980.682 | Schwarz criterion | 5.292688 |
| Log likelihood | -1385.712 | Hannan-Quinn criter. | 5.282820 |
| F-statistic | 103.3627 | Durbin-Watson stat | 1.823686 |
| Prob(F-statistic) | 0.000000 |  |  |

![](img/27e6f65397bb7fd3eced98a716a58f50_353_0.png)

Dependent Variable LOG(WAGE)
Method: Least Squares
Date: 04/19/10  Time: 11:00
Sample: 1 526
Included observations: 526
\(\frac{dy}{y}=\beta_1 \cdot dx\)
\(\frac{\Delta y}{y}=\exp(\beta_1 \cdot \Delta x)-1\)

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 0.583773 | 0.097336 | 5.997510 | 0.0000 |
| EDUC | 0.082744 | 0.007567 | 10.93534 | 0.0000 |
| R-squared | 0.185806 | Mean dependent var | 1.623268 |
| Adjusted R-squared | 0.184253 | S.D. dependent var | 0.531538 |
| S.E. of regression | 0.480079 | Akaike info criterion | 1.374061 |
| Sum squared resid | 120.7691 | Schwarz criterion | 1.390279 |
| Log likelihood | -359.3781 | Hannan-Quinn criter. | 1.380411 |
| F-statistic | 119.5816 | Durbin-Watson stat | 1.801328 |
| Prob(F-statistic) | 0.000000 |  |  |

![](img/27e6f65397bb7fd3eced98a716a58f50_355_0.png)

## 线性一对数模型

- 解释变量每变动1%（相对变化量），引起被解释变量的绝对变化量。

y_i = β_0 + β_1 ln x_i

d(y_i)/d(ln x_i) = d(y_i)/d(x_i)/x_i = β_1

## 例子：GNP与货币供给

Dependent Variable: GNP
Method: Least Squares
Date: 04/19/10  Time: 15:18
Sample: 1973 1987
Included observations: 15

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | -16329.21 | 696.5992 | -23.44133 | 0.0000 |
| LOG(M2) | 2584.785 | 94.04132 | 27.48563 | 0.0000 |
| R-squared | 0.983083 | Mean dependent var | 2790.873 |
| Adjusted R-squared | 0.981782 | S.D. dependent var | 1048.990 |
| S.E. of regression | 141.5874 | Akaike info criterion | 12.86728 |
| Sum squared resid | 260610.9 | Schwarz criterion | 12.96168 |
| Log likelihood | -94.50458 | Hannan-Quinn criter. | 12.86627 |
| F-statistic | 755.4599 | Durbin-Watson stat | 0.595220 |
| Prob(F-statistic) | 0.000000 |  |  |
