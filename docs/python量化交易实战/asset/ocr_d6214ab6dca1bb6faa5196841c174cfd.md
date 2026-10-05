![](bbox=[0.21, 0.06, 0.74, 0.34])

# 图 10-5 201X 年市场 PB-ROE 模型图示

但是对于严重落在 PB-ROE 回归模型曲线下方的标的股票，并不能简单地认为就是被严重低估，而要综合考察其对应当时的基本面和外部条件是否有严重变化，这样才能选择到好的标的。

# 10.3.2 PB-ROE 模型的实现

下面使用掘金量化实现 PB-ROE 模型。

# 第一步：数据表中 PB 值的获取

10.3.1 小节介绍了 PB-ROE 模型的基本概念，对于模型所要使用的数据，掘金量化数据库中有完整的数据存档，在这里直接使用即可。

通过查表可得，ROE 使用的是 ROEAVGCUT，其在 deriv_finance_indicator 中。而 PB 在 trading_derivative_indicator 表中，查询函数如下：

```
def get_fundamentals(table, symbols, start_date, end_date,
fields=None,filter=None, order_by=None, limit=1000, df=False):
```

这里需要注意的是，table 是对应的表名，而 symbols 是批量使用股票列表值同时获取对应的数据。

# 第二步：获取对应股票列表的 PB 值

```
_df = get_fundamentals(table='trading_derivative_indicator',
symbols=symbol_list, start_date=last_day, end_date=last_day,fields="PB",
df=True)
    if len(_df) == len(symbol_list):
        df["PB"] = _df["PB"]
else:
```