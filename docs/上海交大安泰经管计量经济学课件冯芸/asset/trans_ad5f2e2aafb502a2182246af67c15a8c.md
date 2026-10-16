

## 更多的经济金融例子

- 工资和教育
- 选举结果和竞选支出
- 资本资产定价模型（CAPM）
- ……

![](img/27e6f65397bb7fd3eced98a716a58f50_203_0.png)

## 选举结果和竞选支出

- 数据来源：**M. Barone and G. Ujifusa, _The Almanac of American Politics_, 1992. Washington, DC: National Journal.**
- 美国众议院两党竞选的选举结果和竞选支出的数据。每次竞争有两名候选人。
- 被解释变量为候选人A所得票数的百分比，解释变量为候选人A的竞选支出占两位候选人竞选支出总额的百分比。

因变量: VOTEA
方法: 最小二乘
日期: 03/15/10  时间: 17:31
样本: 1 173
包含观测数: 173

| 变量 | 系数 | 标准误 | t统计量 | 概率 |
|---|---|---|---|---|
| C | 26.81221 | 0.887215 | 30.22066 | 0.0000 |
| SHAREA | 0.463827 | 0.014540 | 31.90083 | 0.0000 |
| R-squared | 0.856141 | Mean dependent var | 50.50289 |
| Adjusted R-squared | 0.855300 | S.D. dependent var | 16.78476 |
| S.E. of regression | 6.384840 | Akaike info criterion | 6.557223 |
| Sum squared resid | 6971.018 | Schwarz criterion | 6.593678 |
| Log likelihood | -565.1998 | Hannan-Quinn criter. | 6.572013 |
| F-statistic | 1017.663 | Durbin-Watson stat | 1.826272 |
| Prob(F-statistic) | 0.000000 |  |  |

## CAPM模型的实证检验

E(r_it − r_f) = β · E(r_mt − r_f)

r_it = β₀ + β₁ · r_mt + ε_it，或 E(r_it | ·) = β₀ + β₁ · r_mt

其中，第 i 个股票的收益率 r_it = (P_t + D_t − P_{t−1}) / P_{t−1}

r_mt 是市场收益率（S&P 500 指数）

β₁ 是系统性风险度量指标

数据收集：1986 年 1 月至 1990 年 12 月的月度收益率数据

以 Lincoln National Insurance Corporation 为例

| 变量 | 系数 | 标准误 | t统计量 | 概率 |
|---|---:|---:|---:|---:|
| C | -0.002137 | 0.009078 | -0.235388 | 0.8147 |
| MARKET | 0.973233 | 0.172523 | 5.641188 | 0.0000 |

因变量: LINCOLN  
方法: 最小二乘  
日期: 09/10/11   时间: 13:33  
样本: 1 60  
包含观测数: 60

| 统计量 | 数值 | 统计量 | 数值 |
|---|---:|---|---:|
| R-squared | 0.354286 | Mean dependent var | 0.005075 |
| Adjusted R-squared | 0.343153 | S.D. dependent var | 0.085901 |
| S.E. of regression | 0.069619 | Akaike info criterion | -2.458790 |
| Sum squared resid | 0.281116 | Schwarz criterion | -2.388978 |
| Log likelihood | 75.76369 | Hannan-Quinn criter. | -2.431483 |
| F-statistic | 31.82300 | Durbin-Watson stat | 2.458020 |
| Prob(F-statistic) | 0.000001 | | |

## 三、从一元到多元

$y_i = \hat{\beta}_0 + \hat{\beta}_1 x_{1i} + \hat{\beta}_2 x_{2i} + e_i$

$\min_{\hat{\beta}_0,\hat{\beta}_1,\hat{\beta}_2} \sum e_i^2$

$= \min_{\hat{\beta}_0,\hat{\beta}_1,\hat{\beta}_2} \sum \left(y_i - \hat{\beta}_0 - \hat{\beta}_1 x_{1i} - \hat{\beta}_2 x_{2i}\right)^2$

```
$$\frac{\partial(\sum e_i^2)}{\partial(\hat{\beta}_0)}=0 \qquad \frac{\partial(\sum e_i^2)}{\partial(\hat{\beta}_1)}=0 \qquad \frac{\partial(\sum e_i^2)}{\partial(\hat{\beta}_2)}=0$$
```

```
$$\left\{\begin{aligned}\sum y_i&=\sum(\hat{\beta}_0+\hat{\beta}_1x_{1i}+\hat{\beta}_2x_{2i})\\\sum y_i x_{1i}&=\sum x_{1i}(\hat{\beta}_0+\hat{\beta}_1x_{1i}+\hat{\beta}_2x_{2i})\\\sum y_i x_{2i}&=\sum x_{2i}(\hat{\beta}_0+\hat{\beta}_1x_{1i}+\hat{\beta}_2x_{2i})\end{aligned}\right.$$
```

```
$$\bar{y}=\hat{\beta}_0+\hat{\beta}_1\bar{x}_1+\hat{\beta}_2\bar{x}_2$$

$$\sum e_i\cdot1=0$$

$$\sum e_i x_{1i}=0$$

$$\sum e_i x_{2i}=0$$
```

![](img/27e6f65397bb7fd3eced98a716a58f50_212_0.png)

$$\hat{\beta}_0=\bar{y}-\hat{\beta}_1\bar{x}_1-\hat{\beta}_2\bar{x}_2$$

$$\hat{\beta}_1=\frac{\sum \dot{y}_i\dot{x}_{1i}\sum \dot{x}_{2i}^{2}-\sum \dot{y}_i\dot{x}_{2i}\sum \dot{x}_{1i}\dot{x}_{2i}}{\sum \dot{x}_{1i}^{2}\sum \dot{x}_{2i}^{2}-\left(\sum \dot{x}_{1i}\dot{x}_{2i}\right)^2}$$

$$\hat{\beta}_2=\frac{\sum \dot{y}_i\dot{x}_{2i}\sum \dot{x}_{1i}^{2}-\sum \dot{y}_i\dot{x}_{1i}\sum \dot{x}_{1i}\dot{x}_{2i}}{\sum \dot{x}_{1i}^{2}\sum \dot{x}_{2i}^{2}-\left(\sum \dot{x}_{1i}\dot{x}_{2i}\right)^2}$$

β̂₁ 或 β̂_{y1.2} = [∑ ẏ_i ẋ_{1i} ∑ ẋ_{2i}² - ∑ ẏ_i ẋ_{2i} ∑ ẋ_{1i} ẋ_{2i}] / [∑ ẋ_{1i}² ∑ ẋ_{2i}² - (∑ ẋ_{1i} ẋ_{2i})²] = [∑ ẏ_i ẋ_{1i} / ∑ ẋ_{1i}² - ∑ ẏ_i ẋ_{2i} / ∑ ẋ_{2i}² ∑ ẋ_{1i} ẋ_{2i} / ∑ ẋ_{1i}²] / [1 - (∑ ẋ_{1i} ẋ_{2i})² / (∑ ẋ_{1i}² ∑ ẋ_{2i}²)]
= [y 对 x₁ 回归估计值 - y 对 x₂ 回归估计值 × x₂ 对 x₁ 回归估计值] / (1 - r₁₂²)
= [r_{y1} S_y / S₁ - r_{y2} S_y / S₂ r₁₂ S₂ / S₁] / (1 - r₁₂²)
= (r_{y1} - r_{y2} r₁₂) / (1 - r₁₂²) 或者 = [β̂_{y1} - β̂_{y2} β̂_{21}] / (1 - r₁₂²)
(1 - r₁₂²) S₁ / S_y

《计量经济学》第三章

