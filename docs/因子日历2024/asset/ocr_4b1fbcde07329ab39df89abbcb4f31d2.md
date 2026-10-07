Apr.
4月

![](bbox=[0.356, 0.009, 0.665, 0.202])

星期一
三月廿一

# SemiBeta（负市场收益&正资产收益） 高频因子-波动跳跃类

$$\hat{\beta}_{t,i}^{M-} = -\frac{\sum_{k=1}^{m} r_{t,k,i}^{+} f_{t,k}^{-}}{\sum_{k=1}^{m} f_{t,k}^{2}}$$

$$r_{t,k,i}^{+}=max(r_{t,k,i},0)$$

$$f_{t,k}^{-}=min(f_{t,k},0)$$

# 其中

+   ① $r_{t,k,i}$ 为股票 i 在第 t 期的第 k 个区间的收益率数据；
② $f_{t,k}$ 为市场基准在第 t 期的第 k 个区间的收益率数据，市场基准可以为沪深 300、中证 500 等指数；
③ m 为 t 期内的时间区间数量。

# 说明

基于个股收益和市场基准收益方向的不同，可将传统 Beta 因子拆解为 4 个部分，上述因子即为其中之一，对应市场收益为负但资产收益为正的情况，该因子有相对明显的负溢价。

# 【参考文献】

① Bollerslev, T., Patton, A. J., Quaedvlieg, R., 2022, Realized semibetas: disentangling "good" and "bad" downside risks, Journal of Financial Economics, 144(2), 227-246.
② 安宁宁, 罗军, 蒋原文, 张继东, 2023, 再谈 SemiBeta 因子：高频深层, 广发证券.

![](bbox=[0.425, 0.903, 0.595, 0.949])