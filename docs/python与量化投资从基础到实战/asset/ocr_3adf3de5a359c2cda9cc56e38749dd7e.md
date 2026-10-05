# Python 与量化投资：从基础到实战

在如上所示的表中给出了不同的回撤期，例如从 2016 年 7 月 4 日至 2016 年 7 月 6 日，策略发生了回撤，在 2016 年 7 月 6 日之后策略重新赢利，直到 2016 年 7 月 11 日，组合累计收益率恢复至回撤前的水平。

我们可以_RT调用 plot_drawdown_periods 函数更加直观地对如上所示的表进行展示，其中，参数 returns 为累计收益率序列，top 用于指定需要绘制多少个回撤期（从大到小排列）：

```
fig = plt.figure(figsize=(8, 4))
ax = fig.add_subplot(111)
ax = plot_drawdown_periods(returns, top=1, ax=ax)
```

![](bbox=[0.245, 0.309, 0.775, 0.524])

接下来通过 plot_drawdown_underwater 函数绘制当前时间节点下的策略的回撤：

```
ax = plot_drawdown_underwater(perf['returns'])
```

![](bbox=[0.249, 0.602, 0.771, 0.868])