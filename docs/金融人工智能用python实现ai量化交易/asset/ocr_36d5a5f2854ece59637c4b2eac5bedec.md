```
if self.verbose:
    print(50 * '-')
    print(f'{date} | *** GOING LONG ***')
if self.position == -1:
    self.place_buy_order(bar - 1, units=self.units)
    self.place_buy_order(bar - 1,
                          amount=self.current_balance)
if self.verbose:
    self.print_net_wealth(bar)
self.position = 1
elif self.position in [0, 1] and position == -1:
    if self.verbose:
        print(50 * '-')
        print(f'{date} | *** GOING SHORT ***')
    if self.position = 1:
        self.place_sell_order(bar - 1, units=self.units)
        self.place_sell_order(bar - 1,
                              amount=self.current_balance)
    if self.verbose:
        self.print_net_wealth(bar)
self.position = -1
self.net_wealths.append((self.calculate_net_wealth(price)))        ⑧
self.net_wealths = pd.DataFrame(self.net_wealths,
                                columns=['date', 'net_wealth'])    ⑧
self.net_wealths.set_index('date', inplace=True)                   ⑧
self.net_wealths.index = pd.DatetimeIndex(
                            self.net_wealths.index)                ⑧
self.close_out(bar)
```

+   ① 检索 Finance 环境的状态。
② 根据状态和 model 对象生成最优操作（预测）。
③ 根据最优操作（预测）得到最优头寸（多头 / 空头）。
④ 如果满足条件，就进入多头头寸。
⑤ 如果满足条件，就进入空头头寸。
⑥ 收集随时间变化的净资产值，并将其转换为 DataFrame 对象。

TBBacktester 类的应用很简单，因为 Finance 实例和 TradingBot 实例已经可用。下面的代码首先在**学习环境**数据上对交易机器人进行回测，其中既包括无交易成本的数据也包括有交易成本的数据。图 11-5 直观地比较了这两种情况。

```
In [40]: env = learn_env

In [41]: tb = TBBacktester(env, agent.model, 10000,
                          0.0, 0, verbose=False)          ①

In [42]: tb.backtest_strategy()                             ①
=========================================================
2010-02-05 | *** START BACKTEST ***
2010-02-05 | current balance = 10000.00
=========================================================
```