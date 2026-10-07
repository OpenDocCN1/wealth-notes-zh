# 月度特质波动率

过去 24-60 个月的 Fama-French 三因子回归的残差收益率加权平方和，即：

```
(IV_{i,t}^{WM})^2 = \frac{1}{\sum w_k} \sum_{k=1}^{\tau} \omega_k (\epsilon_{i,t+1-k})^2
```

# 其中

+   ① 权重 $w_k = 0.9^k$；
② $\epsilon_{i,t+1-k}$ 为第 i 个股票在第 t+1-k 月的三因子回归残差。

# 说明

月度特质波动率衡量了股票过去的投机程度，与股票未来收益率呈现显著的负相关。

[参考文献]

> ① Cao, X., & Xu, Y., 2010, Long-run idiosyncratic volatilities and cross-sectional stock returns.

> ② 朱剑滨, 2017, 技术类 Alpha 因子的挖掘测试, 东方证券.