# 12.11 价格变换

我们知道，一般的股票数据都有开盘价、最高价、最低价和收盘价（open、high、low和close），但有时候，我们希望精简价格的信息。比如计算每天的收益率，这样每天就只需要一个价格数据即可，最简单的方法是选取收盘价作为代表，但这样做会丢失不少信息。另外一种方法就是将四种价格进行组合，形成一个新的价格。这就是ta-lib里面的价格变换（Price Transform）。

· AVGPRICE Average Price（均价）

```
real = AVGPRICE(open, high, low, close)
```

这个函数用于对四个价格求平均值。

· MEDPRICE Median Price（中位数价格）

```
real = MEDPRICE(high, low)
```

这个函数用于对最高价、最低价求平均值。

· TYPPRICE Typical Price（代表性价格）

```
real = TYPPRICE(high, low, close)
```