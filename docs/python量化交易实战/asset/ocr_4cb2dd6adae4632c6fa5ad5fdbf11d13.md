```
try:
    _df = get_fundamentals_n(table='deriv_finance_indicator',
symbols=symbol_list[number], end_date=day_time, count=2, fields="OPGPMARGIN")

    now_EBITMARGIN = _df[0]["OPGPMARGIN"]
    last_EBITMARGIN = _df[1]["OPGPMARGIN"]
    EBITG = ((now_EBITMARGIN - last_EBITMARGIN) / last_EBITMARGIN)

    df.iloc[number, 4] = EBITG
except:
    df.iloc[number, 4] = -999

# 求 OPG
for number in range(len(symbol_list)):
    try:
        _df = get_fundamentals_n(table='deriv_finance_indicator',
symbols=symbol_list[number], end_date=day_time, count=2, fields="OPPRORT")

        now_EBITMARGIN = _df[0]["OPPRORT"]
        last_EBITMARGIN = _df[1]["OPPRORT"]
        EBITG = ((now_EBITMARGIN - last_EBITMARGIN) / last_EBITMARGIN)
        df.iloc[number, 5] = EBITG
    except:
        df.iloc[number, 5] = -999

# 求 OCG
for number in range(len(symbol_list)):
    try:
        _df = get_fundamentals_n(table='deriv_finance_indicator',
symbols=symbol_list[number], end_date=day_time, count=2, fields="NCFPS")
        now_EBITMARGIN = _df[0]["NCFPS"]
        last_EBITMARGIN = _df[1]["NCFPS"]
        EBITG = ((now_EBITMARGIN - last_EBITMARGIN) / last_EBITMARGIN)
        df.iloc[number, 6] = EBITG
    except:
        df.iloc[number, 6] = -999

生成的因子矩阵如图 6-7 所示。
```