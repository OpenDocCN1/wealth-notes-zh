![](bbox=[0.13, 0.08, 0.86, 0.38])

图 4-17：一阶矩和二阶矩分别为 0.0 和 1.0 的分布

# 一阶矩和二阶矩

概率分布的一阶矩和二阶矩只能完整描述一个正态分布。有无穷多的其他分布可能与正态分布共享前两个矩，但它们完全不同。

在准备测试真实的金融收益率时，考虑以下 Python 函数，它可以可视化数据为直方图，并在数据的前两个矩上添加一个正态分布的概率密度函数 (PDF)。

```
In [12]: import math
          import scipy.stats as scs
          import statsmodels.api as sm
```

```
In [13]: def dn(x, mu, sigm): ...
        z = (x - mu) / sigm
        pdf = np.exp(-0.5 * z ** 2) / math.sqrt(2 * math.pi * sigm ** 2)
        return pdf
```

```
In [14]: def return_histogram(rets, title=''): ...
        plt.figure(figsize=(10, 6))
        x = np.linspace(min(rets), max(rets), 100)
        plt.hist(np.array(rets), bins=50, density=True, label='frequency')
        y = dn(x, np.mean(rets), np.std(rets))
        plt.plot(x, y, linewidth=2, label='PDF')
        plt.xlabel('log returns')
        plt.ylabel('frequency/probability')
        plt.title(title)
        plt.legend()
```