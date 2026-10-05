```
fill_missing="last")
market_close = msci_tools.get_data_value(market_data,"close")
market_close_ratio = []
for i in range(1,len(market_close)):
    ratio = (market_close[i] - market_close[i-1])/market_close[i-1]
    ratio = (0.99 ** (len(market_close) - i)) * ratio
    market_close_ratio.append(ratio)
symbol_data = history_n(symbol=symbol, frequency="1d", count=count,
    end_time=last_day,
    fields="high,low,close",
    fill_missing="last")
symbol_close = msci_tools.get_data_value(symbol_data,"close")
symbol_close_ratio = []
for i in range(1,len(symbol_close)):
    ratio = (symbol_close[i] - symbol_close[i-1])/symbol_close[i-1]
    ratio = (0.99 ** (len(market_close) - i)) * ratio
    symbol_close_ratio.append(ratio)
    market_close_ratio = sm.add_constant(market_close_ratio)
model_ = sm.OLS(symbol_close_ratio, market_close_ratio)
results_ = model_.fit()
weight_ = (results_.params[1])
return weight_
```

而 Beta 值的计算如下：

#波动率的简单方法

```
def get_volatility_normal(symbol,now,count):
    last_day = get_previous_trading_date("SHSE",now)
    data = history_n(symbol=symbol, frequency="1d", count=count,
end_time=last_day,fields="high,low,close",fill_missing="last")
    close = msci_tools.get_data_value(data,"close")
    res = np.var(close)/np.mean(close)
    res = np.sqrt(res)
    return res
```

这里为了方便起见，波动率采用了收益率的计算方法，而 Beta 值的计算采用了简单的计