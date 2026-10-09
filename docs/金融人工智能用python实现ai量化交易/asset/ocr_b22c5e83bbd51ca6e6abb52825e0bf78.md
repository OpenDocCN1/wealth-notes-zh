```
In [91]: grouped = res.groupby('symbol').mean()
Out[91]: grouped
        mu_cap  mu_real
symbol
AAPL.0  0.118855  0.206158
AMZN.0  0.128223  0.259395
INTC.0  0.117929  0.116180
MSFT.0  0.128844  0.192655
In [92]: grouped.plot(kind='bar', figsize=(10, 6), title='Average Values');
```

![](bbox=[0.19, 0.26, 0.24, 0.33])

# CAPM 的预测能力

相对于市场投资组合而言，CAPM 对股票未来表现的预测能力非常低，甚至对某些股票不具备预测能力。其中一个原因可能是 CAPM 基于与 MVP 理论相同的核心假设，即投资者只关心投资组合和/或股票的（预期）收益率和（预期）波动率。从建模的角度来看，一个假设性的问题是，单个风险因子是否足以解释股票收益率的变化？或者股票收益率和市场投资组合表现之间是否可能存在非线性关系？

# 4.4.4 套利定价理论

从前面的数值例子的结果来看，CAPM 的预测能力似乎相当有限。这里值得质疑的是，仅凭市场投资组合的表现是否足以解释股票收益率的变化。APT 的答案是**否定的**。因为可以有多个（甚至非常多）因子来共同解释股票收益率的可变性。3.5 节正式描述过 APT 的框架，但它也依赖于这些因子和股票收益率之间的线性关系。

数据驱动型投资者认识到，CAPM 不足以可靠地预测股票相对于市场投资组合的表现。因此，投资者决定在市场投资组合中增加 3 个可能推动股票表现的额外因子。

+   - 市场波动率（由 VIX 指数表示，.VIX）
- 汇率（以欧元/美元汇率表示，EUR=）
- 商品价格（以黄金价格表示，XAU=）

下面的 Python 代码实现了一种简单的 APT 方法，它使用 4 个因子结合多元回归方法来解释股票未来的表现。

```
In [93]: Factors = ['.SPX', '.VIX', 'EUR=', 'XAU=']
In [94]: res = pd.DataFrame()
In [95]: np.set_printoptions(formatter={float: lambda x: f"{x:.5f}"})
In [96]: for sym in rets.columns[:4]:
          print('\n' + sym)
          print(71 * '=')
          for year in range(2010, 2019):
              rets_ = rets.loc[f'{year}-01-01':f'{year}-12-31']
              reg = np.linalg.lstsq(rets_[factors],
```