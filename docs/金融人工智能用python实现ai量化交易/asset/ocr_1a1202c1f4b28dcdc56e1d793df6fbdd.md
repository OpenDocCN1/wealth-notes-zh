```
In [38]: sigma(rS)
Out[38]: 0.75

In [39]: sigma(rB)
Out[39]: 0.8

In [40]: np.cov(rM, T, weights=P, ddof=0) ③
Out[40]: array([[0.5625, 0., ],
               [0., 0. ]])
```

+   ① 方差函数。
② 波动率函数。
③ 协方差矩阵。

最后，以等权投资组合为例，投资组合预期收益率、投资组合预期方差和投资组合预期波动率如下。

```
In [41]: phi = np.array((0.5, 0.5))

In [42]: def mu_phi(phi):
            return np.dot(phi, nu(rM)) ①

In [43]: mu_phi(phi)
Out[43]: 0.17500000000000004

In [44]: def var_phi(phi):
            cv = np.cov(rM, T, weights=P, ddof=0)
            return np.dot(phi, np.dot(cv, phi)) ②

In [45]: var_phi(phi)
Out[45]: 0.140625

In [46]: def sigma_phi(phi):
            return var_phi(phi) ** 0.5 ③

In [47]: sigma_phi(phi)
Out[47]: 0.375
```

+   ① 投资组合预期收益率。
② 投资组合预期方差。
③ 投资组合预期波动率。

# 2. 投资机会集

基于蒙特卡罗模拟的投资组合权重 $\phi$ 可以在波动率－收益率空间中可视化投资机会集，如图 3-1 所示。