# 多层订单斜率

高频因子-流动性类

MLQS_weight = (∑ w_i × LogquoteSlope_t^(i)) / (∑ w_i), w_i = i/5, i = 1, 2, 3, 4, 5

LogquoteSlope_t = [log(P_t^A) − log(P_t^B)] / [log(V_t^A) + log(V_t^B)]

# 其中

+   ① P_t^B 和 P_t^A 分别为 t 时刻的买一价和卖一价；
② V_t^B 和 V_t^A 分别为 t 时刻的买一量和卖一量；
③ 对订单价和订单量取对数是为了使序列更接近正态分布，增加因子平稳性；
④ 考虑订单方向：假设卖为正向，买为负向。

# 说明

订单斜率因子衡量的是订单价差对订单量差的敏感度，是对市场流动性的衡量，因子值越大，代表买卖价差越大，市场流动性越差，因此其长期具有流动性风险溢价，长期表现越好。

# [参考文献]

① 丁容明, 汤丞俊, 2021, 多层次订单失衡及订单斜率因子, 中信建投证券.
② Seppi H D J, 2001, Common factors in prices, order flows, and liquidity. Journal of Financial Economics.
设计思路：