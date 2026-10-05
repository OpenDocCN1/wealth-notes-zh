各项参数意义如下。
参数 security：标的代码。
参数 amount：交易数量，正数表示买入，负数表示卖出。
参数 style：下单类型，有两种，分别是市价单（MarketOrder）和限价单（LimitOrder）。市价单，是指不论价格，皆下单，直到交易全部完成。限价单，是指指定一个价格，买入时不能高于它，卖出时不能低于它，如果不满足，则等待满足后再交易。该参数的默认值为 MarketOrder，即 None 代表 MarketOrder。
参数 side：用来指定开多单，还是开空单。其中，long 代表开多单；short 代表开空单。默认为 long，即开多单。需要注意的是，股票和基金暂不支持开空单。
参数 pindex：在使用 set_subportfolios 创建了多个仓位时，指定 subportfolio 的序号，从 0 开始。例如 0 指定第一个 subportfolio，1 指定第二个 subportfolio。该参数.prompt 默认值为 0。
按股数下单函数，如果创建订单成功，则返回 Order 对象；失败则返回 None。按股数下单函数的实例代码如下：

```
# 买入平安银行股票 100 股
order('000001.XSHE', 100)                                          # 下一个市价单
order('000001.XSHE', 100, MarketOrderStyle())       # 下一个市价单，功能同上
order('000001.XSHE', 100, LimitOrderStyle(10.0))   # 以 10 元价格下一个限价单
```

# 7.4.2 目标股数下单函数

目标股数下单函数的语法格式如下：

```
order_target(security, amount, style=None, side='long', pindex=0)
```

目标股数下单函数与按股数下单函数的参数几乎一样，只有第二个参数不一样。目标股数下单函数第二个参数 amount 表示期望的最终持有股票的数量。
目标股数下单函数，如果创建订单成功，则返回 Order 对象；失败则返回 None。目标股数下单函数的实例代码如下：

```
# 卖出平安银行所有股票，即最终持有平安银行的股数为 0
order_target('000001.XSHE', 0)
# 买入平安银行所有股票到 100 股，即最终持有平安银行的股数为 100 股
order_target('000001.XSHE', 100)
```

# 7.4.3 按价值下单函数

按价值下单函数的语法格式如下：