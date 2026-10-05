```
TOTMKTCAP.append(1e+15)

factor_with_table = {}
for line in open("../tools/单因子表列.txt", encoding='UTF-8'):
    line = line.strip('\n')[0:]
    lines = line.split(",")
    factor_with_table[lines[1]] = lines[0]
table_name = factor_with_table[factor]

factor_value_list = []
data = get_fundamentals(table=table_name, symbols=symbol_list,
start_date=last_day,
                    end_date=last_day, fields=factor)

if len(data) == len(symbol_list):
    for _ in data:
        try:
            factor_value_list.append([factor])
        except:
            if more_is_better == True:
                factor_value_list.append(-999)
            else:
                factor_value_list.append(999)
else:
    for symbol in symbol_list:
        try:
            _data = get_fundamentals(table=table_name, symbols=symbol,
start_date=last_day, end_date=last_day, fields=factor)
            factor_value_list.append(_data[0][factor])
        except:
            if more_is_better == True:
                factor_value_list.append(-999)
            else:
                factor_value_list.append(999)

TOTMKTCAP = fast_detai_edge(TOTMKTCAP)  # 这个是 x
factor_value_list = fast_detai_edge(factor_value_list)  # 这个是 y

_TOTMKTCAP = sm.add_constant(TOTMKTCAP)
model = sm.OLS(factor_value_list, _TOTMKTCAP)
results = model.fit()

d_value_dict = {}
```