```
def init(context):
    context.symbol_1 = "SHSE.601939"
    context.symbol_2 = "SHSE.601288"
    context.weight = 0.481477
    context.bias = 0.481829
    context.mean = -0.000203539120501
    context.std = 0.0754807889752
    schedule(schedule_func=algo, date_rule='1d', time_rule='09:31:00')
    context.flagX = False
    context.flagY = False
```

init 函数是初始化数据，在这里对数据进行初始化设定，schedule 是设定函数，规则定义为每日的 9 点 31 分执行算法。而 context.flagX 和 context.flagY 用于对哪只股票标的进行买入。

# 第二步：策略算法的设定

算法的设定较为简单，简单地说，就是当计算差值突破阈值时进行买卖，代码如下：

```
def algo(context):
    last_day = get_previous_trading_date("SHSE", context.now)
    ccb = history(context.symbol_1, frequency="1d", start_time=last_day, end_time=last_day, fields="close", fill_missing="last", adjust=ADJUST_PREV, df=True)
    abchina = history(context.symbol_2, frequency="1d", start_time=last_day, end_time=last_day, fields="close", fill_missing="last", adjust=ADJUST_PREV, df=True)
    ccb = (ccb["close"].values)
    abchina = (abchina["close"].values)
    d_value = abchina - (ccb * context.weight + context.bias)
    if (context.flagX and d_value < context.mean + context.std) or (context.flagY and d_value > context.mean - context.std):
        order_close_all()
        context.flagX = False
        context.flagY = False
    if d_value > context.mean + 3 * context.std:
        order_target_percent(symbol=context.symbol_1, percent=1, position_side=PositionSide_Long, rder_type=OrderType_Market)
```