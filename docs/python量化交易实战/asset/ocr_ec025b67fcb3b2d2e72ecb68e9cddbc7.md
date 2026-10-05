```
if (context.flagX and d_value < context.mean + context.std) or (context.flagY and d_value > context.mean - context.std):
    order_close_all()
    context.flagX = False
    context.flagY = False
if d_value > context.mean + 3 * context.std:
    order_target_percent(symbol=context.symbol_1, percent=1, position_side=PositionSide_Long, order_type=OrderType_Market)
    context.flagX = True
if d_value < context.mean - 3 * context.std:
    order_target_percent(symbol=context.symbol_2, percent=1, position_side=PositionSide_Long, order_type=OrderType_Market)
    context.flagY = True
def on_backtest_finished(context, indicator):
    print(indicator)

if __name__ == "__main__":
    run(
        strategy_id='63e92f59-1386-11e8-bbe9-902b3463caf1',
        filename='配对交易掘金检测.py',
        mode=MODE_BACKTEST,
        token='a71a8083b68e73817e93f7f196b030482abe5939',
        backtest_start_time='2016-01-03 09:00:00',
        backtest_end_time='2016-12-30 15:00:00',
        backtest_initial_cash=100000,
        backtest_adjust=ADJUST_PREV,
        backtest_slippage_ratio=0.01,
        backtest_commission_ratio=0.0005,
    )
```

【程序 11-13】模拟了建仓交易的程序，这里使用的是农业银行与建设银行在 2016 年的回测数据，而参数的确定是根据 2015 年农业银行与建设银行的价差回归曲线设计的。最终结果如图 11-12 所示。