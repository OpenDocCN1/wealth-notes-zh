LYWGE)、个人年龄(AGE)、教育年限(EDUC)、出生年份(Y()B)、出生季度(Q()B)、8 个地区虚拟变量、9 个出生年份虚拟变量等信息。

# 2. Stata 软件中的工具变量估计命令

Stata 软件中的工具变量估计命令为 ivregress，它的 basic 语法和选项如下：

```
ivregress estimator depvar [varlist1] (varlist2 = varlist_iv) [if] [in] [weight] [, options]
```

| estimator | Description |
|---|---|
| 2sls | two-stage least squares (2SLS) |
| liml | limited-information maximum likelihood (LIML) |
| gmm | generalized method of moments (GMM) |

| options | Description |
|---|---|
| SE/Robust |  |
| vce(vcetype) | vcetype may be unadjusted, robust, cluster clustvar, bootstrap. jackknife, or hac kernel |
| Reporting |  |
| level(#) | set confidence level; default is level(95) |
| first | report first-stage estimates |

ivregress 可以采用三种类型的估计量，除了前面介绍的两阶段最小二乘估计量 2sls，还包括有限信息极大似然估计 liml 和广义矩估计 gmm，这里重点考察两阶段最小二乘估计量。depvar 是被解释变量，varlist1 是观测协变量 X_i，为可选項，(varlist2 = varlist_iv) 为必选项，等号左边为内生变量列表，等号右边为工具变量列表，选项 vce(vcetype) 可以选择稳健标准误差 robust、聚类到分组变量 clustvar 的标准误差、自抽样标准误差等。选项 first 可以输出第一阶段估计结果。工具变量回归之后，还有三个相关的命令：estat endogenous、estat firststage、estat overid。ivregress 命令之后可以使用这三个命令进行相关的检验，estat endogenous 可以检验内生性是否存在，estat firststage 报告第一阶段回归的相关统计量，包括 R²、F 检验等，estat overid 可以进行过度识别检验，如果工具变量数多于内生变量数时，在同质性框架下，可以使用过度识别检验检验外生性假设是否成立。

下面，估计一个简单的模型，只有教育一个内生变量，没有任何控制变量，