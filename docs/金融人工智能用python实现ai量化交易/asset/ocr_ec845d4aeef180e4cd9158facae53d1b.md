![](bbox=[0.45, 0.07, 0.6, 0.14])

当 $\beta_{m}=0$ 时，根据 CAPM 公式，预期收益率就是无风险利率。$\beta_{m}$ 越高，风险资产的预期收益率就越高。$\beta_{m}$ 会衡量不可分散的风险。这类风险也称为市场风险或系统性风险。根据 CAPM 的说法，经济主体得到更高预期收益率是承担这一风险的奖励。

# 3.4.2 数值例子

假设静态经济模式有 3 种可能的未来状态，即 $u^{j}=(\{\Omega, x, P_{j}, \Lambda\})$，其中有机会会以无风险利率 $r=0.0025$ 进行借贷。风险资产 $S$ 和 $T$ 的可用数量分别为 0.8 和 0.2。

+   1. 资本市场线

图 3-6 显示了有效边界、市场投资组合、无风险资产和由此产生的风险－收益空间的资本市场线。

```
In [75]: phi_M = np.array((0.8, 0.2))
```

```
In [76]: mu_M = mu_phi(phi_M)
mu_M
Out[76]: 0.10666666666666666
```

```
In [77]: sigma_M = sigma_phi(phi_M)
sigma_M
Out[77]: 0.39474323581566567
```

```
In [78]: r = 0.0025
```

```
In [79]: plt.figure(figsize=(10, 6))
plt.plot(frontier[:, 0], frontier[:, 1], 'm.', ms=5,
         label='efficient frontier')
plt.plot(0, r, 'o', ms=9, label='risk-less asset')
plt.plot(sigma_M, mu_M, '^', ms=10, label='market portfolio')
plt.plot((0, 0.6), (r, r + ((mu_M - r) / sigma_M) * 0.6),
         'r', label='capital market line', lw=2.0)
plt.annotate('$S(\bar{r})$', (0, r), (-0.015, r + 0.01))
plt.annotate('$S(\sigma_{sig\mathrm{m}_{M}})$, ($\mu_{M}\sigma)$, $(\sigma_{\mathrm{m}_{M}}, \mu_{M})$',
         (sigma_M - 0.025, mu_M + 0.01))
plt.xlabel('expected volatility')
plt.ylabel('expected return')
plt.legend();
```