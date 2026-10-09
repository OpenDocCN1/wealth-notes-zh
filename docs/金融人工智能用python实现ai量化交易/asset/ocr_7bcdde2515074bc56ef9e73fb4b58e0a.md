利用前面的数值优化结果，图 3-8 显示了无差异曲线与 CML 相切点处的最优投资组合。可以看出，经济主体确实选择了市场投资组合和无风险资产的组合。

```
In [93]: u = sol[0].subs({'b': 1, 'v': -opt["fun"]})
```

Out[93]:
    u

```
Out[93]:   /                       2
             1 · \/   0.9382296264365521 - sigma
```

```
In [94]: f = lambdify(sigma, u)
```

```
In [95]: u_ = f(sigma_) ⊙
```

```
In [96]: plt.figure(figsize=(10, 6))
     plt.plot(0, r, 'o', ms=9, label='risk-less asset')
     plt.plot(sigma_M, mu_M, '^', ms=9, label='market portfolio')
     plt.plot(opt['x'][1], opt['x'][0], 'v', ms=9, label='optimal portfolio')
     plt.plot((0, 0.5), (r, r + (mu_M - r) / sigma_M * 0.5),
              label='capital market line', lw=2.0)
     plt.plot(sigma_, u_, '-', label='$u=[f$', fmt='round(opt["fun"], 3)))
     plt.xlabel('expected volatility')
     plt.ylabel('expected return')
     plt.legend()
```

+   ① 定义最效用级别的无差异曲线。

+   ② 得到数值以绘制无差异曲线。

![](bbox=[0.15, 0.5, 0.85, 0.8])

图 3-8: CML 上的最优投资组合

本节中的主题通常在**资本市场理论（CMT）**下讨论。CAPM 是该理论的一部分。第 4 章将用实际金融时间序列数据加以说明。