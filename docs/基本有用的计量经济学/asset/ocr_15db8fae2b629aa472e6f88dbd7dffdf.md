```
. ivregress 2sls LWKLYWGE (EDUC = QTR1 QTR2 QTR3), vce(robust)
```

Instrumental variables (2SLS) regression

Number of obs = 329509

Wald chi2(1) = 27.60

Prob > chi2 = 0.0000

R-squared = 0.0937

Root MSE = .64622

| LWKLYWGE | Coef. | Robust Std. Err. | z | P>|z| | [95% Conf. Interval] |
|---|---|---|---|---|---|
| EDUC | .1025976 | .0195281 | 5.25 | 0.000 | .0643233 | .1408719 |
| _cons | 4.589781 | .249385 | 18.40 | 0.000 | 4.100996 | 5.078567 |

Instrumented: EDUC

Instruments: QTR1 QTR2 QTR3

```
. estat overid
```

Test of overidentifying restrictions:

Score chi2(2) = 2.88386 (p = 0.2365)

结果显示，教育收益率仍然为 10.3%，并且很显著。过度识别检验统计量服从自由度为 2 的卡方分布，可以利用卡方分布的临界值，软件已经计算出了相应的 p 值，p 值为 0.2365，远高于 5% 的水平，因而，没有办法拒绝原假设，从而外生性条件 H₀: E[Z; ηi] = 0 可能成立。

# 3. 估计教育收益率

在这一节，我们利用 30 年代出生的个体数据，以出生季度作为工具变量，重现 Angrist and Krueger (1991) 的图 I、图 V 和表 IV。图 I 和图 V 描述的是平均教育年限、平均对数周工资与出生季度之间的关系，先利用 collapse 命令将数据平均到季度：

```
use angrist, clear // 将数据调入系统
```

```
gen year = YOB + (QOB - 1)/4 //产生年份季度顺序变量
```

```
collapse (mean) EDUC LWKLYWGE, by(year) //将教育年限和收入平均到季度
```

```
twoway connected EDUC year, saving(educ, replace) // 画出教育与季度关系图
```

```
twoway connected LWKLYWGE year, saving(lwage, replace) //画出收入与季度关系图
```

上述语句可以重现 Angrist and Krueger (1991) 文中的图 I 和图 V，稍加修改即可以得到图 7.2 和图 7.3。下面，我们复制 Angrist and Krueger (1991) 的表 IV。表 IV 估计了 8 个模型，4 个 OLS 回归，4 个 IV 回归，模型差别主要在于控制的协变量不同，1、2 列仅控制了出生年份，3、4 列进一步控制了年龄及其平