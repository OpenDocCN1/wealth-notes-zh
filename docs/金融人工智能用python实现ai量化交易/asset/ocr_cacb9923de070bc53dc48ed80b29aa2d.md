![](bbox=[0.12, 0.08, 0.84, 0.38])

图 3-1: 模拟投资组合预期波动率和预期收益率（单一风险资产）

因为只有一种风险资产和一种无风险资产，所以机会集是一条直线。

```
In [48]: from pylab import plt, mpl
plt.style.use('seaborn')
mpl.rcParams['savefig.dpi'] = 300
mpl.rcParams['font.family'] = 'serif'
```

```
In [49]: phi_mcs = np.random.random((2, 200)) ¹
```

```
In [50]: phi_mcs = (phi_mcs / phi_mcs.sum(axis=0)).T ²
```

```
In [51]: mcs = np.array([(sigma_phi(phi), mu_phi(phi))
for phi in phi_mcs]) ²
```

```
In [52]: plt.figure(figsize=(10, 6))
plt.plot(mcs[:, 0], mcs[:, 1], 'ro')
plt.xlabel('expected volatility')
plt.ylabel('expected return');
```

+   1. 随机投资组合，标准化为 1。
2. 随机组合的预期投资组合波动率与收益率。

现在设想一种有 3 种状态的静态经济终态，$\omega$。其中 $\Omega = \{u, m, d\}$。这 3 种状态的可能性相等，即 $P = [1/3, 1/3, 1/3]$。可交易资产集合是包括两个固定价格为 $S_0 = T_0 = 10$ 的风险资产 $S$ 和 $T$ 的组合，不确定回报分别为以下各项：