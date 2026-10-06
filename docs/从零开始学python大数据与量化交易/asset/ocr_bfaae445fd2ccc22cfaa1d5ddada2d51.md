# 从零开始学 Python 大数据与量化交易

# 10.2.1 设置基准函数

设置基准函数的语法格式如下：

```
set_benchmark(security)
```

在默认情况下，选定了沪深 300 指数的每日价格作为判断股票量化交易策略好坏和一系列风险值计算的基准。当然也可以使用 set_benchmark 指定其他股票、指数、ETF 的价格作为基准。需要注意的是，这个函数只能在初始化函数（initialize）中调用。

参数 security，是指股票、指数或 ETF 的代码。设置基准函数的返回值为 None。例如，设定 50ETF 基金指数（510050）为基准，其代码如下：

```
set_benchmark("510050.XSHG")
```

再例如，设定深证成指（399001）为基准，其代码如下：

```
set_benchmark("399001.XSHG")
```

还可以设置具体某一只股票，如设定招商银行（600036）为基准，其代码如下：

```
set_benchmark("600036.XSHG")
```

# 10.2.2 设置佣金/印花税函数

设置佣金/印花税函数的语法格式如下：

```
set_order_cost(cost, type, ref=None)
```

设置佣金/印花税函数，用来设定每笔交易要收取的手续费，系统会根据用户指定的费率计算每笔交易的手续费。该函数有 3 个参数，各各参数意义如下。

参数 cost 是 OrderCost 对象，各各项子参数意义如下。

+   - open_tax，买入时印花税（只股票类标的收取，基金与期货不收）。
- close_tax，卖出时印花税（只股票类标的收取，基金与期货不收）。
- open_commission，买入时佣金，申购场外基金的手续费。
- close_commission，卖出时佣金，赎回场外基金的手续费。
- close_today_commission，平今仓佣金。
- min_commission，最低佣金，不包含印花税。

type：指股票（stock）、基金（fund）、金融期货（index_futures）、期货（futures）、债券基金（bond_fund）、股票基金（stock_fund）、QDII 基金（QDII_fund）、货币基金（money_market_fund）、混合基金（mixture_fund）。

ref：参考代码，支持股票代码、基金代码、期货合约代码及期货的品种，如 000001.XSHE、510180.XSHG、IF1809。