# 异常尾部概率S

基于熵原理构造 $S_\varphi$：

```
$S_\varphi = Sign(E_\varphi) * \frac{1}{2}\left\{\int_{-\infty}^{-k}\left(f_1^{\frac{1}{2}} - f_2^{\frac{1}{2}}\right)^2 dx + \int_{k}^{+\infty}\left(f_1^{\frac{1}{2}} - f_2^{\frac{1}{2}}\right)^2 dx\right\}$
```

采用非参的核密度估计法估计未知分布 $f_1(x)$、$f_2(x)$，核密度函数选取高斯核函数：

```
$\widehat{f(x)} = \frac{1}{nh}\sum_{i=1}^{n}\kappa\left(\frac{r_i - x}{h}\right)$

$\kappa(z) = \frac{1}{\sqrt{2\pi}}e^{-\frac{1}{2}z^2}$
```

# 其中

+   ① x 为特质收益率 $\varepsilon_{i,d}$，通过 $R_{i,d}=\alpha_i+\beta_i R_{m,d}+\gamma_i R_{m,d}^2+\varepsilon_{i,d}$ 估计得到；
② k 为左、右异尾的阈值，如 k=1.5；
③ 可以使用过去3个月的日度收益数据计算上述因子。
④ h 为带宽，估计采用 Silverman(1986)经验法则，即 $h\approx1.06\hat{\sigma}n^{-1/5}$。

# 说明

优于偏度的新的收益非对称性的度量方式之一；横截面上股票过去收益正向非对称性越高，未来收益越低；偏度对于未来收益的影响为正或者负与市场风险、股票特质波动率、投资者信心指数、市场流动性以及投资者未实现盈利值有关，如在投资者信心高涨的时期，偏度与未来收益负相关，而在投资者信心低落的时期，偏度与未来收益正相关。

# 【参考文献】

> ① 朱剑涛, 2021, 收益率的非对称分布与尾部蕴含的 Alpha, 东方证券.
② Jiang, L., Wu, K., Zhou, G., & Zhu, Y., 2020, Stock return asymmetry: beyond skewness, Journal of Financial and Quantitative Analysis, 1-65.