```
========== CLOSING OUT ==========
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 18552.00
2019-12-31 | net performance [%] = 5.5200
2019-12-31 | number of trades [#] = 291
=========
```

In [79]: tb.backtest_strategy(sl=None, tsl=0.02,
tp=0.02, wait=5)

```
========== START BACKTEST ==========
2018-01-17 | *** START BACKTEST ***
2018-01-17 | current balance = 10000.00
=========
...
*** TRAILING SL (SHORT | -0.0235) ***
*** TRAILING SL (SHORT | -0.0202) ***
*** TAKE PROFIT (SHORT | 0.0250) ***
*** TAKE PROFIT (SHORT | 0.0227) ***
*** TAKE PROFIT (SHORT | 0.0240) ***
*** TRAILING SL (SHORT | -0.0216) ***
*** TRAILING SL (SHORT | -0.0241) ***
*** TRAILING SL (SHORT | -0.0206) ***
...
========== CLOSING OUT ==========
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 10346.38
2019-12-31 | net performance [%] = 3.4638
2019-12-31 | number of trades [#] = 198
=========
```

+   1. 使用止损单和止盈单对交易机器人的性能进行回测。
2. 使用跟踪止损单和止盈单对交易机器人的性能进行回测。

# 性能影响

风控措施有其合理性和好处。然而，降低风险的代价可能是整体业绩下滑。另外，使用止盈单的回测例子显示了业绩的改善，这可以解释为，给定金融工具的 ATR，某一利润水平是能够实现的。但任何希望看到更高利润的期望，通常都会在市场再次转折时破灭。

# 11.6 结论

本章有 3 个主题。首先以向量化和基于事件的方式对样本外交易机器人（训练有素的深度 Q 学习智能体）的性能进行了回测。然后以 ATR 指标的形式评估了风险，该指标衡量了利率金融工具价格的典型变化。最后，本章讨论并回顾了基于事件的典型风控措施的主语

![](bbox=[0.15, 0.7, 0.24, 0.77])