# 28 3 月 Mar.

星期二
闰二月初七
宣 流动性

# 换手率调整的日成交量为零的数量

流动性因子

```
Lm^x ≡ [ Number of zero daily volume in prior x months + 1/(x - month TO) / Deflator ] 21x / NoTD
```

# 其中

+   ① x - month TO 为过去K个月内，股票的日度换手率之和；
② Deflator = max {1/(x - monthTO)} + 1 为平减指数，在横截面上对所有股票样本中取最大值；
③ NoTD 为过去K个月内的交易日总天数。

# 说明

流动性是风险定价的重要来源，如果股票流动性较差，价格相对较低，投资者对其预期收益就会较高，存在流动性溢价。

[参考文献]
Liu, W, 2006, A liquidity-augmented capital asset pricing model. Journal of Financial Economics 82, 631-671.