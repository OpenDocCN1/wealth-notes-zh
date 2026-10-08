```
1  000001.XSHE  2017-01-04  9.16
2  000001.XSHE  2017-01-05  9.17
3  000001.XSHE  2017-01-06  9.13
4  000001.XSHE  2017-01-09  9.15
After appending:
      secID    tradeDate  closePrice
0   000001.XSHE  2017-01-03        9.16
1   000001.XSHE  2017-01-04        9.16
2   000001.XSHE  2017-01-05        9.17
3   000001.XSHE  2017-01-06        9.13
4   000001.XSHE  2017-01-09        9.15
5   000001.XSHE  2017-01-05        9.17
```

DataFrame 可以像在 SQL 中一样进行合并，在第 5 章中介绍了使用 concat 函数创建 DataFrame 的方法，这是一种合并的方式。另一种方式使用 merge 函数，需要指定依照哪些列进行合并。下列展示了如何根据 security ID 和交易日期合并数据：

```
dat1 = df[['secID', 'tradeDate', 'closePrice']]
dat2 = df[['secID', 'tradeDate', 'turnoverVol']]
dat = dat1.merge(dat2, on=['secID', 'tradeDate'])
print "The first DataFrame:"
print dat1.head()
print "The second DataFrame:"
print dat2.head()
print "Merged DataFrame:"
print dat.head()
```

```
The first DataFrame:
      secID    tradeDate  closePrice
0   000001.XSHE  2017-01-03        9.16
1   000001.XSHE  2017-01-04        9.16
2   000001.XSHE  2017-01-05        9.17
3   000001.XSHE  2017-01-06        9.13
4   000001.XSHE  2017-01-09        9.15
The second DataFrame:
      secID    tradeDate  turnoverVol
0   000001.XSHE  2017-01-03     45984049
1   000001.XSHE  2017-01-04     44932953
2   000001.XSHE  2017-01-05     34437291
3   000001.XSHE  2017-01-06     35815420
4   000001.XSHE  2017-01-09     36108157
Merged DataFrame:
      secID    tradeDate  closePrice  turnoverVol
0   000001.XSHE  2017-01-03        9.16     45984049
1   000001.XSHE  2017-01-04        9.16     44932953
2   000001.XSHE  2017-01-05        9.17     34437291
3   000001.XSHE  2017-01-06        9.13     35815420
4   000001.XSHE  2017-01-09        9.15     36108157
```

DataFrame 另一个强大的函数是 groupby，可以十分方便地对数据进行分组处理。下面对 2017 年 1 月内 10 只股票的开盘价、最高价、最低价、收盘价和成交量求平均值：