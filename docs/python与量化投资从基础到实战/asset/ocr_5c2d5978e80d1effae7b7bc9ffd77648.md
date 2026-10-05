```
ax.plot(assets_cum_value['date'].values, portfolio_cum_value['risk parity'].values, color='r')
    ax.legend([i.decode('utf8') for i in columns]+['risk parity'], prop=font, loc='best')
    ax.xaxis.set_tick_params(labelsize=12)
    ax.yaxis.set_tick_params(labelsize=12)
    ax.grid(True)
```

![](bbox=[0.09, 0.247, 0.905, 0.498])

```
assets_indicators = cal_indicators(selected_daily_return.loc[starts: ends])
    portfolio_indicators = cal_indicators(portfolio_cum_value.pct_change(). dropna())
    assets_indicators['risk parity'] = portfolio_indicators['risk parity']
    assets_indicators
```

| secShortName | 沪深300 | 中小板指 | 恒生指数 | 标普500 | 上证国债 | risk parity |
|---|---|---|---|---|---|---|
| 年化收益率 | 7.53% | 10.43% | 10.61% | 13.74% | 3.02% | 3.89% |
| 年化标准差 | 23.72% | 26.32% | 19.07% | 15.91% | 0.85% | 1.63% |
| 夏普值 | 0.32 | 0.4 | 0.56 | 0.86 | 3.56 | 2.38 |
| 最大回撤 | 52.42% | 55.22% | 37.26% | 22.60% | 1.11% | 2.46% |

从优化结果中我们可以看到，在收益率上风险平价模型并没有体现优势，但风险平价组合收益异常稳健，在兼具国债安全性的同时获得了其他市场带来的一部分收益，其年化标准差、夏普率、最大回撤表现都很优秀。