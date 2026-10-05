```
results = model.fit()
pre_return_profit = results.params[0] + np.asarray(factor_1) *
results.params[1]
```

```
corr = np.corrcoef(pre_return_profit,return_profit)
print(corr)
```

最终结果如下：

```
[[ 1.          0.593641]
[ 0.593641  1.      ]]
```

可以看到，使用两种方法计算出的相关系数的值是相同的。本例中参数的计算相对简单，读者可以采用更为复杂的数据来计算对应的数据值。

补充说明一点，对于不同的股票，由于其市值不同，因子的影响往往差异很大。例如，最基本的市盈率因子，往往会集中于银行等体量比较大的板块中。在对因子的 IC 值进行计算之前，首先要根据不同的条件做回归分析，取残差作为一个替代值。这里不展开介绍，相关代码段如下：

```
def fast_batch_get_neutralized_factor(symbol_list,factor,now,more_is_better):

last_day = get_previous_trading_date("SHSE",now)
_symbol_list = symbol_list
symbol_list = []
for _ in _symbol_list:
    symbol_list.append(_)
TOTMKTCAP = []
#根据市值进行中性化处理
data = get_fundamentals(table='trading_derivative_indicator',
symbols=symbol_list, start_date=last_day, end_date=last_day,
fields="TOTMKTCAP")

    if len(data) == len(symbol_list):
        for _ in data:
            try:
                TOTMKTCAP.append(_["TOTMKTCAP"])
            except:TOTMKTCAP.append(1e+15)
    else:
        for symbol in symbol_list:
            try:
                _data = get_fundamentals(table='trading_derivative_indicator',
symbols=symbol, _date=last_day, end_date=last_day, fields="TOTMKTCAP")
                TOTMKTCAP.append(_data[0]["TOTMKTCAP"])
            except:
```