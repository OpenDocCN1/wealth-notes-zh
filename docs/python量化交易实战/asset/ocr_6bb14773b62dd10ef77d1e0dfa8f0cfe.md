```
context.flagX = True

if d_value < context.mean - 3 * context.std:
    order_target_percent(symbol=context.symbol_2, percent=1, position_side=PositionSide_Long, order_type=OrderType_Market)
    context.flagY = True
```

建仓以后，当差值回归价值曲线后，平仓全部标的。这里需要注意的是，经典的配对交易模型中是对另一个相对标记建空仓，但是由于国内股市对于空仓的建立比较烦琐，这里不建议个人使用，因此略过不写，有兴趣的同学可以自行完成。

# 【程序 11-13】

```
# -*- coding: utf-8 -*-
from gm.api import *

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

def algo(context):
    last_day = get_previous_trading_date("SHSE", context.now)
    ccb = history(context.symbol_1,frequency="1d",start_time=last_day ,end_time=last_day,fields="close",fill_missing="last",adjust=ADJUST_PREV,df=True)
    abchina = history(context.symbol_2, frequency="1d", start_time=last_day, end_time=last_day, fields="close", fill_missing="last", adjust=ADJUST_PREV, df=True)
    ccb = (ccb["close"].values)
    abchina = (abchina["close"].values)
    d_value = abchina - (ccb * context.weight + context.bias)
```