```
df = DataAPI.MktStockFactorsOneDayGet(tradeDate=u"20180119",secID=set_universe("A"),ticker=u"", field=u"secID,tradeDate,ROE",pandas="1")
df.plot.hist(bins=100)
```

![](bbox=[0.175, 0.145, 0.81, 0.393])

图 4-13

```
_ = df.boxplot(sym='rs')
```

![](bbox=[0.175, 0.46, 0.81, 0.785])

图 4-14

这里我们以最常用的 3 倍标准差法为例，将超过 3 倍标准差的数据调整为 3 倍标准差，如图 4-15 所示。