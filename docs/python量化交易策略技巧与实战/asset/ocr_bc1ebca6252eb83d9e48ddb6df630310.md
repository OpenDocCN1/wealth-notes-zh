# 8.9 get_all_securities()函数的运用技巧

get_all_securities()函数可以获取平台支持的所有股票、基金、指数、期货等信息，其语法格式如下：

```
get_all_securities(types=[], date=None)
```

# 8.9.1 各项参数的意义

get_all_securities()函数有两个参数，分别是 types 和 date。

# 1）参数 types

参数 types 是列表类型，用来过滤 securities 的类型，列表元素及意义如下。

+   - stock: 表示股票类型，即显示所有股票信息。\n- fund: 表示基金类型，即显示所有基金信息。\n- index: 表示指数类型，即显示所有指数信息。\n- futures: 表示期货类型，即显示所有期货信息。\n- etf: 表示 ETF 基金，即显示所有 ETF 基金信息。\n- lof: 表示 LOF 基金，即显示所有 LOF 基金信息。\n- fja: 表示分级 A，即显示所有分级基金的 A 份额的信息。\n- fjb: 表示分级 B，即显示所有分级基金的 B 份额的信息。\n- open_fund: 表示开放式基金，即显示所有开放式基金信息。\n- bond_fund: 表示债券基金，即显示所有债券基金信息。\n- stock_fund: 表示股票型基金，即显示所有股票型基金信息。\n- QDII_fund: 表示 QDII 基金，即显示所有 QDII 基金信息。\n- money_market_fund: 表示货币基金，即显示所有货币基金信息。\n- mixture_fund: 表示混合型基金，即显示所有混合型基金信息。

需要注意的是，types 为空时，返回所有股票信息，但不包括基金、指数和期货信息。

# 2）参数 date

参数 date 是日期类型，一个字符串或 datetime.datetime/datetime.date 对象，用于获取某日期还在上市的股票信息，默认值为 None，表示获取所有日期的股票信息。

get_all_securities()函数的返回值是 pandas.DataFrame 类型。