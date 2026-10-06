```
def check_stocks(context):
    # 设定查询条件
    q = query(
        valuation.code,
        valuation.market_cap
    ).filter(
        valuation.market_cap.between(300,600)
    ).order_by(
        valuation.market_cap.asc()
    )
    # 选出低市值的股票，构成buylist
    df = get_fundamentals(q)
    buylist = list(df['code'])
    # 过滤停牌股票
    buylist = filter_paused_stock(buylist)
    return buylist[:g.stocknum]
```

# 16.8.3 编写过滤停牌股票函数

过滤停牌股票函数，具体代码如下：

```
def filter_paused_stock(stock_list):
    current_data = get_current_data()
    return [stock for stock in stock_list if not current_data[stock].paused]
```

# 16.8.4 编写交易函数

交易函数，即每天开盘时买入，持有 5 个交易日，然后调仓，具体代码如下：

```
def trade(context):
    if g.days%g.refresh_rate == 0:
        ## 获取持仓列表
        sell_list = list(context.portfolio.positions.keys())
        # 如果有持仓，则卖出
        if len(sell_list) > 0 :
            for stock in sell_list:
                order_target_value(stock, 0)
        ## 分配资金
        if len(context.portfolio.positions) < g.stocknum :
            Num = g.stocknum - len(context.portfolio.positions)
            Cash = context.portfolio.cash/Num
        else:
            Cash = 0
        ## 选股
        stock_list = check_stocks(context)
```