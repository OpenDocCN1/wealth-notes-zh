![](bbox=[0.108, 0.06, 0.837, 0.246])

# 图 7-5 不同斜率（Beta 值）对应市场走势

RSRS 因子的基本计算代码如下：

```
def get_rsrs_weight_not_support(symbol, now, length = 600, window = 18):
    last_day = get_previous_trading_date("SZSE", now)
    last_last_day = get_previous_trading_date("SZSE", last_day)
    data = history_n(symbol=symbol, frequency="1d", count=length + window,
                    end_time=last_last_day,
                    fields="open,high,low,close",
                    fill_missing="last", df=True)
    data_high = (data["high"].values)
    data_low = (data["low"].values)
    #这里是直到前天的 RSRS 序列值
    rsrs_weights = []
    for len_ in range(len(data) - window + 1):
        high_ = data_high[len_:len_ + window]
        low_ = data_low[len_:len_ + window]
        low_ = sm.add_constant(low_)
        model_ = sm.OLS(high_, low_)
        results_ = model_.fit()
        weight_ = (results_.params[1])
        # rsrs_weights.append(weight_)  #原始 weight
        rsrs_weights.append(weight_)  # 这里是加上权重的 weight
    last_day_data = history_n(symbol=symbol, frequency="1d", count=window,
    end_time=last_day,
                    fields="open,high,low,close", fill_missing="last",
    df=True)
    high_ = last_day_data["high"].values
    low_ = last_day_data["low"].values
```