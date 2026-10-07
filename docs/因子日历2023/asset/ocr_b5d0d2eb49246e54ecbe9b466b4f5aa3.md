# 带条件的流动性冲击

流动性因子

```
ILLIQ_{i,t} = α_{0,i} + α_{1,i} ILLIQ_{i,t-1} + α_{2,i} ε_{i,t-1} + ε_{i,t}
```

# 其中

+   ① ILLIQ_{i,t} 为股票 i 在 t 月的 Amihud 非流动性因子；
② 每个月滚动过去 60 个月的 Amihud 非流动性因子序列拟合上述 ARMA(1,1) 模型，估计方法采用极大似然法，至少需要包含 24 个月的样本数据；
③ ARMA(1,1) 估计得到的 -ε_{i,t} 即为带条件的流动性冲击。

# 说明

带条件的流动性冲击为真实的非流动性与估计得到的非流动性条件均值之差再取相反数，是流动性冲击的一种带参数的衡量方法，与股票未来收益呈反比。

> [参考文献]
Turan G. Bali, Lin Peng, Yannan Shen, Yi Tang, 2014, Liquidity Shocks and Stock Market Reactions, The Review of Financial Studies 27(5):1434-1485.