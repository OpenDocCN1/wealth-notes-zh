
5. 检查是否为多头头寸给出了一个跟踪止损事件。
6. 检查是否为空头头寸给出了一个跟踪止损事件。

正如上面的回测结果显示，与没有跟踪止损单的策略相比，在给定参数的情况下使用跟踪止损单会降低总体性能。

```python
In [75]: tb.backtest_strategy(sl=None, tsl=0.015, tp=None, wait=5)
2018-01-17 | *** START BACKTEST ***
2018-01-17 | current balance = 10000.00

*** TRAILING SL (SHORT | -0.0152) ***
*** TRAILING SL (SHORT | -0.0169) ***
*** TRAILING SL (SHORT | -0.0164) ***
*** TRAILING SL (SHORT | -0.0191) ***
*** TRAILING SL (SHORT | -0.0116) ***
*** TRAILING SL (SHORT | -0.0194) ***
*** TRAILING SL (SHORT | -0.0172) ***
*** TRAILING SL (SHORT | -0.0181) ***
*** TRAILING SL (SHORT | -0.0153) ***
*** TRAILING SL (SHORT | -0.0160) ***
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 10577.93
2019-12-31 | net performance [%] = 5.7793
2019-12-31 | number of trades [#] = 201
```

1. 用跟踪止损单对交易机器人的性能进行回测。

### 11.5.3 止盈

最后是止盈单。一个止盈单将平仓达到一定利润水平的头寸。假设一个非杠杆多头头仓以100的价格开仓，止盈单被设置为5%的水平。如果价格达到105，则平仓。

下面`TbBacktester`类的代码最终显示了处理止盈单的部分。给定止损单和跟踪止损单代码的引用，止盈实现是很简单的。对于止盈单，可以选择使用与相关高/低价格水平相比的保证价格水平进行回测，但这很可能导致性能远低于乐观。

注3：止盈单有一个固定的目标价格水平。因此，用一个时间间隔的高价来计算多头头寸或选用这个时间间隔的低价来计算空头头寸是不现实的。

```python
# 止盈单
if tp is not None and self.position != 0:
    rc = (price - self.entry_price) / self.entry_price
    if self.position == 1 and rc > self.tp:
        print(50 * '-')
        if guarantee:
            price = self.entry_price * (1 + self.tp)
            print(f'*** TAKE PROFIT (LONG | {self.tp:.4f}) ***')
        else:
            print(f'*** TAKE PROFIT (LONG | {rc:.4f}) ***')
            self.place_sell_order(bar, units=self.units, gprice=price)
        self.wait = wait
        self.position = 0
    elif self.position == -1 and rc < -self.tp:
        print(50 * '-')
        if guarantee:
            price = self.entry_price * (1 - self.tp)
            print(f'*** TAKE PROFIT (SHORT | {self.tp:.4f}) ***')
        else:
            print(f'*** TAKE PROFIT (SHORT | {-rc:.4f}) ***')
            self.place_buy_order(bar, units=self.units, gprice=price)
        self.wait = wait
        self.position = 0
```

与被动投资相比，在给定的参数条件下，增加一个无保证金的止盈单可以显著提高交易机器人的性能。考虑到之前必须考虑的事，这个结果可能过于乐观了。因此，在这种情况下，带保证金的止盈单使其性能值更加现实。

```python
In [76]: tb.backtest_strategy(sl=None, tp=0.015, wait=5, guarantee=False)
=================================================
2018-01-17 | *** START BACKTEST ***
2018-01-17 | current balance = 10000.00
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0155) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0155) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0204) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0240) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0168) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0156) ***
-------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0183) ***
=================================================
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 11210.33
2019-12-31 | net performance [%] = 12.1033
2019-12-31 | number of trades [#] = 190
=================================================
```

```python
In [77]: tb.backtest_strategy(sl=None, tsl=None, tp=0.015, wait=5, guarantee=True)
2018-01-17 | *** START BACKTEST ***
2018-01-17 | current balance = 10000.00
======================================================================
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0150) ***
======================================================================
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 10980.86
2019-12-31 | net performance [%] = 9.8086
2019-12-31 | number of trades [#] = 198
======================================================================
```

1. 用一个止损单对交易机器人的性能进行回测（无保证金）。
2. 用一个止损单对交易机器人的性能进行回测（有保证金）。

当然，止损单/跟踪止损单也可以和止盈单合并。下面的 Python 代码的回测结果在这两种情况下都比没有风控措施的策略结果更糟糕。在风险管理方面，几乎没有“免费的午餐”。

```python
In [78]: tb.backtest_strategy(sl=0.015, tsl=None, tp=0.015, wait=5)
2018-01-17 | *** START BACKTEST ***
2018-01-17 | current balance = 10000.00
======================================================================
*** STOP LOSS (SHORT | -0.0203) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0202) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0213) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0240) ***
------------------------------------------------------------------
*** STOP LOSS (SHORT | -0.0171) ***
------------------------------------------------------------------
*** TAKE PROFIT (SHORT | 0.0188) ***
------------------------------------------------------------------
*** STOP LOSS (SHORT | -0.0153) ***
------------------------------------------------------------------
*** STOP LOSS (SHORT | -0.0154) ***
```

```
========== CLOSING OUT ==========
2019-12-31 | *** CLOSING OUT ***
2019-12-31 | current balance = 18552.00
2019-12-31 | net performance [%] = 5.5200
2019-12-31 | number of trades [#] = 291
=========
```

```python
In [79]: tb.backtest_strategy(sl=None, tsl=0.02, tp=0.02, wait=5)
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

1. 使用止损单和止盈单对交易机器人的性能进行回测。
2. 使用跟踪止损单和止盈单对交易机器人的性能进行回测。

### 性能影响

风控措施有其合理性和好处。然而，降低风险的代价可能是整体业绩下滑。另外，使用止盈单的回测例子显示了业绩的改善，这可以解释为，给定金融工具的 ATR，某一利润水平是能够实现的。但任何希望看到更高利润的期望，通常都会在市场再次转折时破灭。

## 11.6 结论

本章有 3 个主题。首先以向量化和基于事件的方式对样本外交易机器人（训练有素的深度 Q 学习智能体）的性能进行了回测。然后以 ATR 指标的形式评估了风险，该指标衡量了利率金融工具价格的典型变化。最后，本章讨论并回顾了基于事件的典型风控措施的主观。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_294_0.png)

单、跟踪止损单和止盈单形式。

与自动驾驶汽车类似，交易机器人很少只基于人工智能的预测来部署。为了避免巨大的下行风险并提高（风险调整后的）业绩，风控措施通常会发挥作用。本章所讨论的标准风控措施既适用于大多数交易平台，也适用于散户交易者。第 12 章将以 Oanda 交易平台为例说明这一点。基于事件的回溯方法提供了算法上的灵活性，可以正确地回溯此类风控措施的效果。虽然“降低风险”听起来很吸引人，但回溯结果表明，降低风险往往是有代价的；与设有任何风控措施的纯策略相比，性能可能更低。然而，当进行精细调试时，结果显示，像止盈单这样的风控措施也可以对性能产生积极影响。

## 11.7 Python代码

### 11.7.1 金融环境

下面是带有 Finance 环境类的 Python 模块。

```python
# 金融环境
# (c) Dr. Yves J. Hilpisch
# Artificial Intelligence in Finance
#
import math
import random
import numpy as np
import pandas as pd

class observation_space:
    def __init__(self, n):
        self.shape = (n,)

class action_space:
    def __init__(self, n):
        self.n = n

    def sample(self):
        return random.randint(0, self.n - 1)

class Finance:
    intraday = False
    if intraday:
        url = 'http://hilpisch.com/aif_etkon_id_eur_usd.csv'
    else:
        url = 'http://hilpisch.com/aif_etkon_eod_data.csv'
    def __init__(self, symbol, features, window, lags,
                leverage=1, min_performance=0.85, min_accuracy=0.5,
                start=0, end=None, mu=None, std=None):
```
