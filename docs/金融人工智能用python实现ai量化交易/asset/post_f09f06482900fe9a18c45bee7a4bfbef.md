
In [74]: res = pd.DataFrame()
for year in range(2010, 2019):
    rets_ = rets[symbols].loc[f'{year}-01-01':f'{year}-12-31']
    epv = port_volatility(rets_, opt_weights[year]) ①
    epr = port_return(rets_, opt_weights[year]) ①
    esr = epr / epv ①

    rets_ = rets[symbols].loc[f'{year + 1}-01-01':f'{year + 1}-12-31']
    rpv = port_volatility(rets_, opt_weights[year]) ②
    rpr = port_return(rets_, opt_weights[year]) ②
    rsr = rpr / rpv ②

    res = res.append(pd.DataFrame({'epv': epv, 'epr': epr, 'esr': esr,
                                  'rpv': rpv, 'rpr': rpr, 'rsr': rsr,
                                  index=[year + 1]}))

In [75]: res
Out[75]:

| | epv | epr | esr | rpv | rpr | rsr |
|:---|:---|:---|:---|:---|:---|:---|
| 2011 | 0.157440 | 0.303003 | 1.924564 | 0.160622 | 0.133836 | 0.833235 |
| 2012 | 0.173279 | 0.169321 | 0.977156 | 0.182292 | 0.161375 | 0.885256 |
| 2013 | 0.202460 | 0.278459 | 1.375378 | 0.168714 | 0.166897 | 0.989228 |
| 2014 | 0.181544 | 0.368961 | 2.032353 | 0.177798 | 0.026830 | 0.135645 |
| 2015 | 0.160340 | 0.309486 | 1.930190 | 0.211368 | -0.024560 | -0.116194 |
| 2016 | 0.326730 | 0.778330 | 2.382179 | 0.296565 | -0.103870 | -0.350242 |
| 2017 | 0.106148 | 0.090933 | 0.856663 | 0.079521 | 0.230630 | 2.900235 |
| 2018 | 0.086548 | 0.260702 | 3.012226 | 0.157337 | 0.036324 | 0.230804 |
| 2019 | 0.323796 | 0.220808 | 0.704174 | 0.207672 | 0.275819 | 1.328147 |

In [76]: res.mean()
Out[76]: epv    0.199020
         epr    0.309689
         esr    1.688320
         rpv    0.184654
         rpr    0.123659
         rsr    0.838755
dtype: float64

- ① 投资组合预期的统计值。
- ② 投资组合实际的统计值。

图4-6比较了单年的预期与实际的投资组合波动率。MVP理论在预测投资组合波动率方面做得相当好。两个时间序列之间相对较高的相关性也支持了这一点。

In [77]: res[['epv', 'rpv']].corr()
Out[77]:        epv         rpv
epv     1.000000    0.765733
rpv     0.765733    1.000000

In [78]: res[['epv', 'rpv']].plot(kind='bar', figsize=(10, 6),
                    title='Expected vs. Realized Portfolio Volatility');

![](img/75a6cddadc3157a8dd9222cb5ba456ca_124_0.png)

图 4-6：预期与实际的投资组合波动率

然而，当预期与实际的投资组合收益率进行比较时，结论则正好相反（参见图 4-7）。MVP 理论显然无法预测投资组合的收益率，两个时间序列之间的负相关性证实了这一点。

In [79]: res[['epr', 'rpr']].corr()
Out[79]:
          epr          rpr
epr  1.000000 -0.350437
rpr -0.350437  1.000000

In [80]: res[['epr', 'rpr']].plot(kind='bar', figsize=(10, 6),
            title='Expected vs. Realized Portfolio Return');

![](img/75a6cddadc3157a8dd9222cb5ba456ca_124_1.png)

图 4-7：预期与实际的投资组合收益率

对夏普比率来说，我们会得出类似甚至更糟的结论（参见图 4-8）。以投资组合夏普比率最大化为目标的数据驱动型投资者来说，该理论的预测值通常与实际值相差很大。两个时间序列之间的相关性甚至比收益率之间的相关性还要低。

In [81]: res[['esr','rsr']].corr()
Out[81]:         esr        rsr
esr   1.000000 -0.698607
rsr  -0.698607  1.000000

In [82]: res[['esr','rsr']].plot(kind='bar', figsize=(10,6),
             title='Expected vs. Realized Sharpe Ratio');

![](img/75a6cddadc3157a8dd9222cb5ba456ca_125_0.png)

图 4-8：预期与实际的投资组合夏普比率

## MVP 理论的预测能力

![](img/75a6cddadc3157a8dd9222cb5ba456ca_125_1.png)

将 MVP 理论应用到真实世界的数据中揭示了其实际的不足。如果没有额外的约束条件，那么最优的投资组合的构成和再平衡可能是很极端的状态。在数值例子中，对投资组合收益率和夏普比率的预测能力是相当差的，而对投资组合风险的预测能力似乎是可以接受的。然而，投资者通常感兴趣的是风险调整后的业绩衡量指标，比如夏普比率，而在这个例子中，MVP 理论从统计上来说是最不适用的。

### 4.4.3 资本资产定价模型

类似的方法也可以应用于 CAPM 的实际测试。假设前面所讲的数据驱动型投资者希望应用 CAPM 来获得之前的 4 只科技股的预期收益率。下面的 Python 代码会首先求出给定年份的每只股票的 beta 系数，然后根据该股票的 beta 系数和市场投资组合的表现，计算其明年的预期收益率。市场投资组合由标准普尔 500 指数近似计算。

In [83]: r = 0.005①
In [84]: market = '.SPX'②
In [85]: rets = np.log(raw / raw.shift(1)).dropna()
In [86]: res = pd.DataFrame()
In [87]: for sym in rets.columns[:4]:
  print('\n' + sym)
  print(54 * '=')
  for year in range(2010, 2019):
    rets_ = rets.loc[f'{year}-01-01':f'{year}-12-31']
    muH = rets_[market].mean() * 252
    cov = rets_.cov()
    beta = cov.loc[sym, market]③
    var = rets_[market].var()④
    rets_ = rets.loc[f'{year + 1}-01-01':f'{year + 1}-12-31']
    muH = rets_[market].mean() * 252
    mu_capm = r + beta * (muH - r)⑤
    mu_real = rets_[sym].mean() * 252⑥
    res = res.append(pd.DataFrame({'symbol': sym,
                    'mu_capm': mu_capm,
                    'mu_real': mu_real},
                    index=[year + 1]),
                    sort=False)
  print('{:} | beta: {.3f} | mu_capm: {.63f} | mu_real: {.63f}'.format(year + 1, beta, mu_capm, mu_real)⑥

- ① 指定无风险短期利率。
- ② 定义市场投资组合。
- ③ 求出股票的 beta 系数。
- ④ 根据上一年的 beta 系数和当年的市场投资组合表现计算预期收益率。
- ⑤ 计算当年股票的实际业绩。
- ⑥ 汇集并打印所有结果。

以上代码的输出如下。

| 股票 | 年份 | beta | mu_capm | mu_real |
|:---|:---|:---|:---|:---|
| AAPL.O | 2011 | 1.052 | -0.000 | 0.228 |
| AAPL.O | 2012 | 0.764 | 0.098 | -0.275 |
| AAPL.O | 2013 | 1.266 | 0.327 | 0.053 |
| AAPL.O | 2014 | 0.630 | 0.070 | 0.320 |
| AAPL.O | 2015 | 0.833 | -0.005 | -0.047 |
| AAPL.O | 2016 | 1.144 | 0.103 | 0.096 |
| AAPL.O | 2017 | 1.009 | 0.180 | 0.381 |
| AAPL.O | 2018 | 1.379 | -0.091 | -0.071 |
| AAPL.O | 2019 | 1.252 | 0.316 | 0.621 |

MSFT.O
========== ============= ============= =============
2011 | beta: 0.890 | mu_capm: 0.001 | mu_real: 0.072
2012 | beta: 0.816 | mu_capm: 0.104 | mu_real: 0.029
2013 | beta: 1.109 | mu_capm: 0.287 | mu_real: 0.337
2014 | beta: 0.876 | mu_capm: 0.095 | mu_real: 0.216
2015 | beta: 0.955 | mu_capm: -0.007 | mu_real: 0.178
2016 | beta: 1.249 | mu_capm: 0.113 | mu_real: 0.113
2017 | beta: 1.224 | mu_capm: 0.217 | mu_real: 0.321
2018 | beta: 1.303 | mu_capm: 0.086 | mu_real: 0.172
2019 | beta: 1.442 | mu_capm: 0.364 | mu_real: 0.440

INTC.O
========== ============= ============= =============
2011 | beta: 1.081 | mu_capm: -0.008 | mu_real: 0.142
2012 | beta: 0.842 | mu_capm: 0.108 | mu_real: -0.163
2013 | beta: 1.081 | mu_capm: 0.280 | mu_real: 0.230
2014 | beta: 0.883 | mu_capm: 0.096 | mu_real: 0.335
2015 | beta: 1.055 | mu_capm: -0.008 | mu_real: -0.052
2016 | beta: 1.009 | mu_capm: 0.092 | mu_real: 0.051
2017 | beta: 1.261 | mu_capm: 0.223 | mu_real: 0.242
2018 | beta: 1.163 | mu_capm: 0.076 | mu_real: 0.017
2019 | beta: 1.376 | mu_capm: 0.347 | mu_real: 0.243

AMZN.O
========== ============= ============= =============
2011 | beta: 1.102 | mu_capm: -0.081 | mu_real: -0.039
2012 | beta: 0.958 | mu_capm: 0.122 | mu_real: 0.374
2013 | beta: 1.116 | mu_capm: 0.289 | mu_real: 0.464
2014 | beta: 1.262 | mu_capm: 0.335 | mu_real: -0.251
2015 | beta: 1.473 | mu_capm: -0.013 | mu_real: 0.778
2016 | beta: 1.122 | mu_capm: 0.102 | mu_real: 0.104
2017 | beta: 1.118 | mu_capm: 0.199 | mu_real: 0.446
2018 | beta: 1.300 | mu_capm: -0.086 | mu_real: 0.251
2019 | beta: 1.619 | mu_capm: 0.408 | mu_real: 0.207

图 4-9 比较了在考虑到上一年的 beta 系数和当年的市场投资组合表现时，单只股票的预期（期望）收益率与该股票本年的实际收益率。显然，CAPM 的原始形式在仅基于 beta 系数预测股票表现时并没有真正发挥作用。

In [88]: sym = 'AMZN.O'

In [89]: res[res['symbol'] == sym].corr()
Out[89]:
             mu_capm    mu_real
mu_capm    1.000000   0.004826
mu_real    -0.004826   1.000000

In [90]: res[res['symbol'] == sym].plot(kind='bar',
                     figsize=(10, 6), title=sym)

![](img/75a6cddadc3157a8dd9222cb5ba456ca_128_0.png)

### 图 4-9：单只股票的 CAPM 预期收益率与实际股票收益率

图 4-10 比较了 CAPM 预期股票收益率的平均值与实际收益率的平均值。在这里，CAPM 同样表现不佳。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_128_1.png)

### 图 4-10：多只股票的 CAPM 预期平均收益率与实际股票平均收益率

很容易看到，CAPM 对所分析股票的预期平均收益率影响不大，在 12.2% 和 14.4% 之间。然而，股票的实际平均收益率表现出了很高的差异性，这一比例在 9.4% 和 29.2% 之间。仅凭市场投资组合表现和 beta 系数之间的关系显然不能解释（科技）股票已观察到的收益率。

In [91]: grouped = res.groupby('symbol').mean()
Out[91]: grouped
        mu_capm  mu_real
symbol
AAPL.O  0.118855  0.206158
AMZN.O  0.128223  0.259395
INTC.O  0.117929  0.116180
MSFT.O  0.128844  0.192655

In [92]: grouped.plot(kind='bar', figsize=(10, 6), title='Average Values');

![](img/75a6cddadc3157a8dd9222cb5ba456ca_129_0.png)

## CAPM 的预测能力

相对于市场投资组合而言，CAPM 对股票未来表现的预测能力非常低，甚至对某些股票不具备预测能力。其中一个原因可能是 CAPM 基于与 MVP 理论相同的核心假设，即投资者只关心投资组合和/或股票的（预期）收益率和（预期）波动率。从建模的角度来看，一个假设性的问题是，单个风险因子是否足以解释股票收益率的变化？或者股票收益率和市场投资组合表现之间是否可能存在非线性关系？

### 4.4.4 套利定价理论

从前面的数值例子的结果来看，CAPM 的预测能力似乎相当有限。这里值得质疑的是，仅凭市场投资组合的表现是否足以解释股票收益率的变化。APT 的答案是**否定的**。因为可以有多个（甚至非常多）因子来共同解释股票收益率的可变性。3.5 节正式描述过 APT 的框架，但它也依赖于这些因子和股票收益率之间的线性关系。

数据驱动型投资者认识到，CAPM 不足以可靠地预测股票相对于市场投资组合的表现。因此，投资者决定在市场投资组合中增加 3 个可能推动股票表现的额外因子。

- 市场波动率（由 VIX 指数表示，.VIX）
- 汇率（以欧元/美元汇率表示，EUR=）
- 商品价格（以黄金价格表示，XAU=）

下面的 Python 代码实现了一种简单的 APT 方法，它使用 4 个因子结合多元回归方法来解释股票未来的表现。

In [93]: Factors = ['.SPX', '.VIX', 'EUR=', 'XAU=']
In [94]: res = pd.DataFrame()
In [95]: np.set_printoptions(formatter={float: lambda x: f"{x:.5f}"})
In [96]: for sym in rets.columns[:4]:
          print('\n' + sym)
          print(71 * '=')
          for year in range(2010, 2019):
              rets_ = rets.loc[f'{year}-01-01':f'{year}-12-31']
              reg = np.linalg.lstsq(rets_[factors],
