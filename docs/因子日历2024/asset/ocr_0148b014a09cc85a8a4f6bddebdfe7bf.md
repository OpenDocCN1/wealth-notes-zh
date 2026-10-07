Dec.
12月

![](bbox=[0.355, 0.008, 0.665, 0.203])

星期五
冬月十三

# SemiBeta（负市场收益&负资产收益） 高频因子-波动跳跃类

```
$$\hat{\beta}_{t,i}^{N} = \frac{\sum_{k=1}^{m} r_{t,k,i}^{-} f_{t,k}^{-}}{\sum_{k=1}^{m} f_{t,k}^{2}}$$
```

```
$$r_{t,k,i}^{-} = min(r_{t,k,i}, 0)$$
```

```
$$f_{t,k}^{-} = min(f_{t,k}, 0)$$
```

# 其中

+   ① $r_{t,k,i}$ 为股票 i 在交易日 t 内第 k 个区间的收益率数据；
② $f_{t,k}$ 为市场基准在交易日 t 内第 k 个区间的收益率数据，市场基准可以为沪深 300、中证 500 等指数；
③ m 为交易日 t 内的时间区间数量。

# 说明

基于个股收益和市场基准收益方向的不同，可将传统 Beta 因子拆解为 4 个部分，上述因子即为其中之一，对应市场收益和资产收益同为负的情况，在 A 股市场，该因子的负溢价更明显，但在美股市场却相反。

> 【参考文献】
① Bollerslev, T., Patton, A. J., Quaedvlieg, R., 2022, Realized semibetas: disentangling "good" and "bad" downside risks, Journal of Financial Economics, 144(2), 227-246.
② 安宁宁, 罗军, 漆原文, 张绪东, 2023, 再谈 SemiBeta 因子：高频测算, 广发证券.