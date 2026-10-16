

## 一、什么是“好”模型

- 简约性：简单优于复杂
- 可识别性：参数估计值惟一
- 拟合优度：越高越好
- 理论一致性
- 预测能力

![](img/27e6f65397bb7fd3eced98a716a58f50_336_0.png)

## 导致模型失效的可能之一：
模型函数形式的选择

- 根据数据/问题本身的特征确定
  - 对数 vs. 水平值
    - （对严格为正的变量）取对数的好处
      - 斜率项系数不随变量测量单位变化而变化
      - 在一定程度上消除异方差
      - 在一定程度上降低参数估计值对变量极端观测值的敏感性
  - 双曲函数模型
  - 多项式函数模型

Equation: EQ01 Workfile: CEOSAL1::Ceosal1\
Dependent Variable: SALARY
Method: Least Squares
Date: 10/16/11 Time: 17:10
Sample: 1 209
Included observations: 209

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 963.1913 | 213.2403 | 4.516930 | 0.0000 |
| ROE | 18.50119 | 11.12325 | 1.663290 | 0.0978 |

R-squared 0.013189 Mean dependent var 1281.120
Adjusted R-squared 0.008421 S.D. dependent var 1372.345
S.E. of regression 1366.555 Akaike info criterion 17.28750
Sum squared resid 3.87E+08 Schwarz criterion 17.31948
Log likelihood -1804.543 Hannan-Quinn criter. 17.30043
F-statistic 2.766532 Durbin-Watson stat 2.104990
Prob(F-statistic) 0.097768

Equation: EQ02 Workfile: CEOSAL1::Ceosal1\
Dependent Variable: SALARY
Method: Least Squares
Date: 10/16/11 Time: 17:11
Sample: 1 209 IF SALARY<10000
Included observations: 207

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 949.5851 | 111.2878 | 8.532698 | 0.0000 |
| ROE | 12.71692 | 5.811438 | 2.188256 | 0.0298 |

R-squared 0.022825 Mean dependent var 1167.628
Adjusted R-squared 0.018059 S.D. dependent var 719.5987
S.E. of regression 713.0716 Akaike info criterion 15.98666
Sum squared resid 1.04E+08 Schwarz criterion 16.01886
Log likelihood -1652.619 Hannan-Quinn criter. 15.99968
F-statistic 4.788465 Durbin-Watson stat 1.903751
Prob(F-statistic) 0.029781

## Equation: EQ01 Workfile: CEOSAL1::Ceosal1\

Dependent Variable: LOG(SALARY)
Method: Least Squares
Date: 10/16/11 Time: 17:12
Sample: 1 209
Included observations: 209

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 6.712169 | 0.086644 | 77.46792 | 0.0000 |
| ROE | 0.013863 | 0.004520 | 3.067190 | 0.0024 |

| R-squared | 0.043472 | Mean dependent var | 6.950386 |
| Adjusted R-squared | 0.038851 | S.D. dependent var | 0.566374 |
| S.E. of regression | 0.555263 | Akaike info criterion | 1.670774 |
| Sum squared resid | 63.82163 | Schwarz criterion | 1.702758 |
| Log likelihood | -172.5958 | Hannan-Quinn criter. | 1.683705 |
| F-statistic | 9.407652 | Durbin-Watson stat | 1.886133 |
| Prob(F-statistic) | 0.002449 | | |

## Equation: EQ02 Workfile: CEOSAL1::Ceosal1\

Dependent Variable: LOG(SALARY)
Method: Least Squares
Date: 10/16/11 Time: 17:12
Sample: 1 209 IF SALARY>10000
Included observations: 207

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 6.710187 | 0.078243 | 85.76039 | 0.0000 |
| ROE | 0.012592 | 0.004086 | 3.081850 | 0.0023 |

| R-squared | 0.044279 | Mean dependent var | 6.926088 |
| Adjusted R-squared | 0.039617 | S.D. dependent var | 0.511577 |
| S.E. of regression | 0.501341 | Akaike info criterion | 1.466556 |
| Sum squared resid | 51.52534 | Schwarz criterion | 1.498756 |
| Log likelihood | -149.7885 | Hannan-Quinn criter. | 1.479577 |
| F-statistic | 9.497798 | Durbin-Watson stat | 1.714415 |
| Prob(F-statistic) | 0.002340 | | |

## 例子：价格弹性的度量

需求的价格弹性 = dy/dx · x/y = αβ₁ · x^{β₁-1} · x/(αx^{β₁}) = β₁

![](img/27e6f65397bb7fd3eced98a716a58f50_340_0.png)

![](img/27e6f65397bb7fd3eced98a716a58f50_340_1.png)

## 对数线性模型（或双对数模型，又称为不变弹性模型）

$$y=\alpha x^{\beta_1}$$

$$\ln y_i=\ln\alpha+\beta_1\ln x_i$$

$$y_i^*=\beta_0+\beta_1x_i^*$$

$$y_i^*=\ln y_i，\quad x_i^*=\ln x_i，\quad \beta_0=\ln\alpha$$

Dependent Variable: LOG(DEMAND)
Method: Least Squares
Date: 04/19/10  Time: 09:39
Sample: 1 10
Included observations: 10

| Variable | Coefficient | Std. Error | t-Statistic | Prob. |
|---|---|---|---|---|
| C | 3.961701 | 0.041588 | 95.26059 | 0.0000 |
| LOG(PRICE) | -0.227146 | 0.025010 | -9.082100 | 0.0000 |
| R-squared | 0.911587 | Mean dependent var | 3.618610 |
| Adjusted R-squared | 0.900535 | S.D. dependent var | 0.174391 |
| S.E. of regression | 0.054999 | Akaike info criterion | -2.786131 |
| Sum squared resid | 0.024200 | Schwarz criterion | -2.725614 |
| Log likelihood | 15.93065 | Hannan-Quinn criter. | -2.852518 |
| F-statistic | 82.48453 | Durbin-Watson stat | 0.876532 |
| Prob(F-statistic) | 0.000017 | | |

