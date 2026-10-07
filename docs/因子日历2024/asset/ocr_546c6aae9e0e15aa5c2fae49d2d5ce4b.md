Jun.
6月

![](bbox=[0.35, 0.008, 0.66, 0.201])

星期六
五月初十

# 上下行跳跃波动的不对称性

高频因子-波动跳跃类

$SRVJ_t = RVLP_t - RVJN_t$

$RVJP_t = max(RS_t^+ - \frac{1}{2}\widehat{IV}_t, 0)$

$RVJN_t = max(RS_t^- - \frac{1}{2}\widehat{IV}_t, 0)$

# 其中

RVJP_t 和 RVJN_t 分别为交易日 t 的上行跳跃波动率和下行跳跃波动率，计算细节可参考相关日历页。

# 说明

将上行跳跃波动率减下行跳跃波动率即可得到上下行跳跃波动的不对称性；短期内，跳跃波动的不对称性通常有负的风险溢价，股价短期大幅上涨，未来更易补跌，股价短期大幅下跌，未来更易补涨。

【参考文献】
Bo Yu, Bruce Mizrach, Norman R. Swanson, 2020, New Evidence of the Marginal Predictive Content of Small and Large Jumps in the Cross-Section, Econometrics, MDPI, 8(2), 1-52.