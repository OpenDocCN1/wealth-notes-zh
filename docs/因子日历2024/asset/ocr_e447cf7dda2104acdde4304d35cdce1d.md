# 已实现跳跃波动率

高频因子-波动跳跃类

$RVJ_t = max(RV_t - \widehat{IV}_t, 0)$

$RV_t = \sum_{i=1}^{n} r_{i,n}^2 \to_u QV_t = \int_0^t \sigma_s^2 ds + \sum_{s \leq t} \Delta X_s^2 = IV_t + QJ_t$

$\widehat{IV}_t = \mu_{\frac{2}{3}}^{-3} \sum_{i=k}^{n} |r_{t_i}|^{\frac{2}{3}} |r_{t_{i-1}}|^{\frac{2}{3}} |r_{t_{i-k+1}}|^{\frac{2}{3}}$

# 其中

+   ① $r_{t_i}$ 为某股票交易日 t 内的分钟对数收益率序列，如 5 分钟对数收益率序列；
② k=3，n 表示该股票在交易日 t 中特定频率的分钟级别数据个数，如 5 分钟频率通常有 48 个数据点；
③ $\mu_q$ 为标准正态分布的第 q 阶绝对矩；
④ 已实现跳跃波动率 RVJ 整体取值为正，所以需要用 max 对负差值做修正。

# 说明

已实现跳跃波动率刻画了股价波动率中股价跳跃部分的波动水平，可通过已实现波动剔除积分波动率估计得到；跳跃波动率衡量了股价在短时间内发生非连续性、剧烈变动（股价跳跃行为）的波动程度，跳跃行为能较好地动态反映市场情绪和信息冲击。

# [参考文献]

+   ① Barndorff-Nielsen, O. E. and N. Shephard, 2004, Power and bipower variation with stochastic volatility and jumps, Journal of Financial Econometrics, 2, 1-48.
② Andersen, T. G., Bollerslev, T., Diebold, F. X., 2007, Roughing it up: Including jump components in the measurement, modeling, and forecasting of return volatility, Review of Economics and Statistics, 89, 701-720.
③ Bo Yu, Bruce Mizrach, Norman R. Swanson, 2020, New Evidence of the Marginal Predictive Content of Small and Large Jumps in the Cross-Section, Econometrics, MDPI, 8(2), 1-52.