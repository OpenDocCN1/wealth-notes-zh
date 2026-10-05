```
for number in range(len(symbol_list)):
    try:
        _df = get_fundamentals(table='trading_derivative_indicator',
symbols=symbol_list[number], start_date=last_day,end_date=last_day,
fields="PB")
        _factor_value = tools.get_data_value(_df, "PB")
        df.iloc[number, 1] = _factor_value[0]
    except:
        df.iloc[number, 1] = np.mean(df["PB"])
```

这里首先使用 get_fundamentals 函数读取对应的股票列表的 PB 值，之后对读取的数据进行确认，这样做的目的是防止数据产生确实值，如果测试中的数据长度不等于股票列表的长度，就使用 for 循环单独获取对应的 PB 值，如果产生确实值，就使用已有列表的均值对其实行填充。

而 ROE 数据的获取与 PB 获取一样，代码段如下：

```
_df = get_fundamentals(table='deriv_finance_indicator', symbols=symbol_list,
start_date=last_day, end_date=last_day, fields="ROEAVG", df=True)
    if len(_df) == len(symbol_list):
        df["ROE"] = _df["ROEAVG"]
    else:
        for number in range(len(symbol_list)):
            try:
                _df = get_fundamentals(table='deriv_finance_indicator',
symbols=symbol_list[number], start_date=last_day,end_date=last_day,
fields="ROEAVG")
                _factor_value = tools.get_data_value(_df, "ROEAVG")
                df.iloc[number, 2] = _factor_value[0]
            except:
                df.iloc[number, 2] = np.mean(df["ROE"])
```

下面使用前面介绍的 OLS 方法对数据进行回归分析，代码如下：

```
df = df.dropna()
pb_ = df["PB"].values # 这是 Y
roe_ = df["ROE"].values # 这是 X

roe = []
for _ in roe_:
    roe.append(_)
pb = []
for _ in pb_:
```