接下来，利用 for 循环和 if 条件语句，判断股票代码是否在 stockset[:g.stocknum]中，如果不在，就调用 order_target_value 函数卖出股票，具体代码如下：

```
for stock in sell_list:
    if stock not in stockset[:g.stocknum]:
        stock_sell = stock
        # 如果股票代码不在 stockset[:g.stocknum]中，则调用 order_target_value
        # 函数卖出股票
        order_target_value(stock_sell, 0)
```

接下来，判断持仓股票个数是否小于 g.stocknum，具体代码如下：

```
# 如果持仓股票个数小于 g.stocknum
if len(context.portfolio.positions) < g.stocknum:
    # num 为 g.stocknum 减去持仓股票个数
    num = g.stocknum - len(context.portfolio.positions)
    # cash 为账户当前现金除以 num
    cash = context.portfolio.cash/num
else:
    # 如果持仓股票个数大于 g.stocknum，则 cash 和 num 均为 0
    cash = 0
    num = 0
```

如果持仓股票个数少于 5，则从 stockset[:g.stocknum]中买进股票，即调用 order_target_value 函数买进股票，具体代码如下：

```
for stock in stockset[:g.stocknum]:
    if stock in sell_list:
        pass
    else:
        stock_buy = stock
        # 如果持仓股票个数少于 5, 此时就会从 stockset[:g.stocknum]中买进股票
        # 即调用 order_target_value 函数买进股票
        order_target_value(stock_buy, cash)
        # 减少 num 减 1
        num = num - 1
        # 如果 num 为 0, 则退出 for 循环
        if num == 0:
            break
```

最后全局变量 g.days 加 1，具体代码如下：

```
g.days += 1    # 全局变量 g.days 加 1
else:
    g.days = g.days + 1
```