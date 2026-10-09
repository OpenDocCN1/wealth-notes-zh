11.7.3 节会介绍 BacktestingBase 类，它可以灵活地用于测试不同类型的方向性交易策略。代码的重要行上有详细的注释。此类提供了以下方法。

get_date_price()

对于给定的 bar（包含金融数据的 DataFrame 对象的索引值），该方法会返回相关的 date 和 price。

print_balance()

对于给定的 bar，该方法会打印交易机器人的当前（现金）余额。

calculate_net_wealth()

对于给定的 price，该方法会返回由当前（现金）余额和工具头寸组成的净资产。

print_net_wealth()

对于给定的 bar，该方法会打印交易机器人的净资产。

place_buy_order(), place_sell_order()

对于给定的 bar 和给定的 units 数量或给定的 amount，这两种方法会发出买单或卖市单，并相应地调整相关的数量（比如，计算交易成本）。

close_out()

在给定的 bar 上，该方法会关闭未平仓头寸，并计算和报告业绩统计数据。

下面的 Python 代码演示了 BacktestingBase 类函数的实例如何基于一些简单的步骤运行。

```
```
In [29]: import backtesting as bt
In [30]: bb = bt.BacktestingBase(env=agent.learn_env, model=agent.model,
          amount=10000, ptc=0.0001, ftc=1.0, verbose=True) ①

In [31]: bb.initial_amount ②
Out[31]: 10000

In [32]: bar = 100 ③

In [33]: bb.get_date_price(bar) ④
Out[33]: ('2010-06-25', 1.2374)

In [34]: bb.env.get_state(bar) ⑤
Out[34]: EUR= r s m v
          Date
          2010-06-22  0.0242  0.5622  0.0916  0.2022  1.5316
          2010-06-23  0.0176  0.6940  0.0939  0.0915  1.5563
          2010-06-24  0.0354  0.3034  0.0865  0.6391  1.8090

In [35]: bb.place_buy_order(bar, amount=5000) ⑥
          2010-06-25 | buy 4040 units for 1.2374
          2010-06-25 | current balance = 4999.40

In [36]: bb.print_net_wealth(2 * bar) ⑦
          2010-11-16 | net_wealth = 10459.17
```
```