# 。定义：

```
$$\hat{\theta}_K(r) = \frac{\hat{\mu}_{tp} - r}{\hat{\sigma}_{tp}} \tag{2.72}$$
```

它表示这条切线的斜率。由于不同的（0, r）点会产生不同的切线，因此$\hat{\theta}_K(r)$是$r$的函数。当$r$等于无风险利率$R_f$时，$\hat{\theta}_K(R_f)$恰恰就是从（0, $R_f$）出发得到的切点组合的夏普比率（Sharpe 1966a）。

![](bbox=[0.26, 0.22, 0.72, 0.44])

图 2.9 $\hat{\theta}_K(r)$ 的定义

当把$N$个资产加入后，使用全部$N+K$个资产张成最小方差前沿并按类似式（2.72）的方式定义$\hat{\theta}_{N+K}(r)$。利用$\hat{\theta}_K(r)$和$\hat{\theta}_{N+K}(r)$便可得到$s_1$和$s_2$的表达式：

```
$$s_1 = \max_r \frac{1 + \hat{\theta}_{N+K}^2(r)}{1 + \hat{\theta}_K^2(r)} - 1 \tag{2.73}$$
```

```
$$s_2 = \min_r \frac{1 + \hat{\theta}_{N+K}^2(r)}{1 + \hat{\theta}_K^2(r)} - 1 \tag{2.74}$$
```

最后，通过$s_1$和$s_2$求出似然比检验、Wald检验以及拉格朗日乘数检验的统计量（分别记为$LR$、$W$和$LM$）：

```
$$LR = T\left(\ln(1+s_1) + \ln(1+s_2)\right) \stackrel{A}{\sim} \chi^2_{2N} \tag{2.75}$$
```

```
$$W = T\left(s_1 + s_2\right) \stackrel{A}{\sim} \chi^2_{2N} \tag{2.76}$$
```

```
$$LM = T\left(\frac{s_1}{1+s_1} + \frac{s_2}{1+s_2}\right) \stackrel{A}{\sim} \chi^2_{2N} \tag{2.77}$$
```

这三种检验的统计量虽然略有差异，但它们都是以某种形式将$s_1$和$s_2$“加”起来作为一个综合的分数来检验原假设的。由$s_1$和$s_2$的定义可知，人们实际上是在均值一方差平面的纵轴上搜寻两个特殊的$r$。对于第一个$r$，由$K$和$N+K$个资产张成的最