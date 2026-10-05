```
order_target_percent(symbol=context.index, percent=1,
order_type=OrderType_Market,
    position_side=PositionSide_Long)

elif macd[-1] < 0 and macd[-2] > 0:
    order_target_percent(symbol=context.index, percent=0,
order_type=OrderType_Market,
    position_side=PositionSide_Long)

def on_backtest_finished(context, indicator):
    print(indicator)
if __name__ == "__main__":
    run(
        strategy_id='9f53e15f-6870-11e8-801c-4cedfb681747',
        filename=(os.path.basename(__file__)),
        mode=MODE_BACKTEST,
        token='715f84a04a4574e2e77491d62f7c8bbc0c663b12',
        backtest_start_time='2016-01-05 09:30:00',
        backtest_end_time='2018-06-23 15:00:00',
        backtest_initial_cash=10000000,
        backtest_adjust=ADJUST_PREV
    )
```

这里的核心程序是使用 MACD 对收盘价进行判定，当 MACD 线上穿 0 线时买入，当 MACD 下穿 0 线时卖出，而采用的股票对象是沪深 300 指数，这样可以另类地作为一种测试择时的判定条件。

结果如图 7-3 所示。

![](bbox=[0.05, 0.56, 0.9, 0.87])

图7-3 基于自定义 MACD 的择时

可以看到，基于自定义的 MACD 能够在一定程度上做到择时，但是数据并不值得使