子进行排序，买入排名前 5 的标的内容。

```
def algo(context):
    now = context.now

    order_close_all()

symbol_list= get_history_constituents(index=context.index, start_date=now)[0].get("constituents").keys()
    df = get_fundamentals_n(table="deriv_finance_indicator",
symbols=symbol_list, end_date=now, fields="NPGRT",count=1, df=True)
    df = df.sort_values(["NPGRT"], ascending=False)

    target_list = df["symbol"].values
    target_list = target_list[:context.num]

    for symbol in target_list:
        order_target_percent(symbol=symbol, percent=1. / context.num,
order_type=OrderType_Market, position_side=PositionSide_Long)
```

这里 order_close_all 为了简略起见，直接清仓。在真实的实盘交易时，为了节省交易费用等，需要判断持仓情况，这里先不采用。

# 【程序 6-2】

```
from gm.api import *
import os

def init(context):
    context.index = "SHSE.000016"
    context.num = 5
    schedule(schedule_func=algo, date_rule='1m', time_rule='09:31:00')
def algo(context):
    now = context.now
    order_close_all()

symbol_list= get_history_constituents(index=context.index, start_date=now)[0].get("constituents").keys()
    df = get_fundamentals_n(table="deriv_finance_indicator",
symbols=symbol_list, end_date=now, fields="NPGRT",count=1, df=True)
    df = df.sort_values(["NPGRT"], ascending=False)

    target_list = df["symbol"].values
    target_list = target_list[:context.num]
```