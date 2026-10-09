在以上输出报表中，包含了以下金融指标参数。

+   • Total Return，总回报率。

+   • Risk-free rate，无风险利率

+   • Sharp Rate，夏普指数。

+   • Sortino Rate，索提诺指数

+   • Calmar Ratio，卡尔马比率。

+   • CAGR，年复合增长率。

+   • Max Drawdown，最大回撤。

+   • MTD、YTM，月度、年度指标。

+   • Mean，平均成交量。

+   • Vol，平均交易金额。

+   • Best Day(Monthly)，最佳交易日（月）。

+   • Worst Day(Monthly)，最差交易日（月）。

+   • Win，获利。

其中，主要的金融指标参数，如夏普指数、索提诺指数、最大回撤等，还提供了年均、月均、日均三种不同时间周期的指数。

第 8.2 组代码如下：

```
```python
xcod='002046'
print('\n#8.2 display monthly_return')
m1=perf[xcod].display_monthly_returns()
print(m1)
```
```

根据股票代码，计算单只股票各个年度的月均回报率。以下是案例中 A 股 002046 对应的月均回报率。

| Year | Jan | Feb | Mar | Apr | May | Jun | Jul | Aug | Sep | Oct | Nov | Dec | YTD |
|------|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| 2017 | 13.36 | 6.9 | -1.29 | -14.24 | -17.89 | 13.1 | -12.32 | 7.03 | -2.42 | 0 | 0 | 0 | -12.77 |

第 8.3 组代码如下：

```
```python
print('\n#8.3 xcod.stats')
m2=perf[xcod].stats
print(m2)
```
```