# Dimson贝塔

波动率因子

星期二

五月廿四

7 月 Jul.

宣 波动率

```
r_{id} - r_{fd} = α_i + β_{i1} (r_{md-1} - r_{fd-1}) + β_{i2} (r_{md} - r_{fd}) + β_{i3} (r_{md+1} - r_{fd+1}) + ε_{id}
```

```
betaDM = β̂_{i1} + β̂_{i2} + β̂_{i3}
```

# 其中

+   ① r_{id}、r_{md}、r_{fd} 分别为K个月内第 d 日的股票收益率、市场收益率、无风险利率；
② 一般K = 1、6、12个月，至少包含15个日度收益率。

# 说明

作者通过同时引入领先和滞后的收益率做回归的形式来处理股票非频繁交易带来的贝塔估计偏差问题。

> 【参考文献】
Dimson, Elroy,1979, Risk measurement when shares are subject to infrequent trading, Journal of Financial Economics 7, 197-226.