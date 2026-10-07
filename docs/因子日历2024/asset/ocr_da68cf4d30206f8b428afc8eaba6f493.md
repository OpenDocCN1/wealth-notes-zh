# 大的上下行跳跃波动不对称性

高频因子-波动跳跃类

```
SRVLJ_t = RVLJP_{γ,t} - RVLJN_{γ,t}
```

```
RVLJN_{γ,t} = min(RVJN_t, Σ_{i=1}^n r_{i,t}^2 I_{r_{i,t} < -γ})
```

```
RVLJP_{γ,t} = min(RVJP_t, Σ_{i=1}^n r_{i,t}^2 I_{r_{i,t} > γ})
```

# 其中

RVLJP_{γ,t} 和 RVLJN_{γ,t} 分别为交易日 t 的大的上行跳跃波动率和大的下行跳跃波动率，计算细节可参考相关日历页。

# 说明

跳跃波动率可以根据阈值进一步分解为大、小两个部分，即用于区分信息冲击导致的大跳跃波动和无法用信息冲击解释的小跳跃波动；大的上下行跳跃波动不对称性主要对应大跳跃波动的不对称性。

# 【参考文献】

① Duong, D., Swanson, N. R., 2015, Empirical evidence on the importance of aggregation, asymmetry, and jumps for volatility prediction, Journal of Econometrics, 187, 606-621.

② Bo Yu, Bruce Mizrach, Norman R. Swanson, 2020, New Evidence of the Marginal Predictive Content of Small and Large Jumps in the Cross-Section, Econometrics, MDPI, 8(2), 1-52.