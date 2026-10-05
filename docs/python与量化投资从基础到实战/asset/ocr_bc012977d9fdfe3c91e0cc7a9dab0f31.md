加上后缀 XSHG。当然，也支持直接传入股票代码，可以将原始股票的代码传入 ticker 参数，如图 4-4 所示。

```
DataAPI.MktEqudGet(tradeDate=u"20150513",secID=["000001.XSHE","600000.XSHG"],
    ticker=u"",field=u"secID,secShortName,tradeDate,closePrice,marketValue",pandas="1")
```

|  | secID | secShortName | tradeDate | closePrice | marketValue |
| 0 | 000001.XSHE | 平安银行 | 2015-05-13 | 15.88 | 217712795944 |
| 1 | 600000.XSHG | 浦发银行 | 2015-05-13 | 17.07 | 318414756798 |

图 4-3

```
DataAPI.MktEqudGet(tradeDate=u"20150513",secID=u"",ticker=
["000001","600000"],field=u"secID,secShortName,tradeDate,closePrice,marketValue",pandas="1")
```

|  | secID | secShortName | tradeDate | closePrice | marketValue |
| 0 | 000001.XSHE | 平安银行 | 2015-05-13 | 15.88 | 217712795944 |
| 1 | 600000.XSHG | 浦发银行 | 2015-05-13 | 17.07 | 318414756798 |

图 4-4

除了某一天的截面数据，DataAPI 也支持获取过去一时间段的数据，通过 beginDate 与 endDate 参数即可方便地获取。如图 4-5 所示，我们调取了平安银行与浦发银行从 2018 年 1 月 17 日至 2018 年 1 月 19 日的收盘价与总市值数据。

```
DataAPI.MktEqudGet(tradeDate=u"",secID=u"",ticker=["000001","600000"],
beginDate='20180117',endDate='20180121',
field=u"secID,secShortName,tradeDate,closePrice,marketValue",pandas="1")
```

|  | secID | secShortName | tradeDate | closePrice | marketValue |
| 0 | 000001.XSHE | 平安银行 | 2018-01-17 | 14.23 | 244334954222 |
| 1 | 000001.XSHE | 平安银行 | 2018-01-18 | 14.72 | 252748455880 |
| 2 | 000001.XSHE | 平安银行 | 2018-01-19 | 14.80 | 254122088720 |
| 3 | 600000.XSHG | 浦发银行 | 2018-01-17 | 13.10 | 384512253240 |
| 4 | 600000.XSHG | 浦发银行 | 2018-01-18 | 13.24 | 388621544496 |
| 5 | 600000.XSHG | 浦发银行 | 2018-01-19 | 13.24 | 388621544496 |

图 4-5

股票日行情 DataAPI 支持提取多只股票在某一时间段的数据，我们再来看看量化投资常用的因子数据，如图 4-6 所示。