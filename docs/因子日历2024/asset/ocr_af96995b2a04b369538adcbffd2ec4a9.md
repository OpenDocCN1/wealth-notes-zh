# 小的上行跳跃波动率

RVSJP_t = RVJP_t - RVLJP_{γ,t}

RVLJP_{γ,t} = min(RVJP_t, sum_{i=1}^{n} r_{i,t}^2 I_{ {r_{i,t}>γ} })

RVJP_t = max( RS_t^+ - 1/2 \widehat{IV}_t, 0 )

γ = α Δ_n^{0.49} \sqrt{\widehat{IV}_t}

# 其中

+   ① RVJN_t 和 RVLJN_{γ,t} 分别为交易日 t 的上行跳跃波动率和大的上行跳跃波动率，计算细节可参考相关日历页；
② γ 为区分大、小跳跃的阈值，Δ_n 为日内分钟数据个数的倒数，如 5 分钟频率的 Δ_n = 1/48，经验参数 α = 4；
③ r_{t_i} 为某股票交易日 t 内的分钟对数收益率序列，如 5 分钟对数收益率序列。

# 说明

跳跃波动率可以根据阈值进一步分解为大、小两个部分，即用于区分信息冲击导致的大跳跃波动和无法用信息冲击解释的小跳跃波动，上行波动率也可以做上述划分；由于小跳跃波动通常由短期流动性风险导致，投资者预期并未改变，对未来收益的预测能力要弱于大跳跃波动。

【参考文献】
① Duong, D., Swanson, N. R., 2015, Empirical evidence on the importance of aggregation, asymmetry, and jumps for volatility prediction, Journal of Econometrics, 187, 606-621.
② Bo Yu, Bruce Mizrach, Norman R. Swanson, 2020, New Evidence of the Marginal Predictive Content of Small and Large Jumps in the Cross-Section, Econometrics, MDPI, 8(2), 1-52.

![](bbox=[0.36, 0.01, 0.68, 0.21])

Aug. 8月

星期日 七月初一

高频因子-波动跳跃类