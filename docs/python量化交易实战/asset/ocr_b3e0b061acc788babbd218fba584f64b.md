```
from MSCI_tools import 计算 RSRS as RSRS
def sell_check_rsrs(context,symbol):
    threshold = -0.7
    now = context.now
    zscore_rightdev = RSRS.get_rsrs_weight_classic(symbol,now)

    if zscore_rightdev < threshold:
        return True
    else:
        return False

（8）庄股值计算（庄股值：能够无量涨停的或无量杀跌的）：

def cow_stock_value(context, symbol):
    now = context.now
    _pb = get_fundamentals(table='trading_derivative_indicator', symbols=symbol, start_date=now,
                        end_date=now, fields='PB,NEGOTIABLEMV')
    pb = msci_tools.get_data_value(_pb, "PB")
    NEGOTIABLEMV = (msci_tools.get_data_value(_pb, "NEGOTIABLEMV"))[0]/100000000

    if NEGOTIABLEMV > 100:
        return 0
    num_fall = fall_money_day_3line(context, symbol, 120, 20, 60, 160)
    num_cross = money_5_cross_60(context, symbol, 120, 5, 160)
    return (num_fall * num_cross) / (pb * (NEGOTIABLEMV ** 0.5))

（9）3 条移动平均线计算缩量：

def fall_money_day_3line(context,symbol,n,n1=20,n2=60,n3=120):
    now = context.now
    last_day = get_previous_trading_date("SHSE",now)

    data = history_n(symbol, frequency="1d", count=n+n3, end_time=last_day, fields="cum_volume")
    stock_m = msci_tools.get_data_value(data,"cum_volume")
    i=0
    count=0
    while i<n:
        money_MA200=np.mean(stock_m[i:n3-1+i])
        money_MA60=np.mean(stock_m[i+n3-n2:n3-1+i])
        money_MA20=np.mean(stock_m[i+n3-n1:n3-1+i])
        if money_MA20<=money_MA60 and money_MA60<=money_MA200:
            count=count+1
```