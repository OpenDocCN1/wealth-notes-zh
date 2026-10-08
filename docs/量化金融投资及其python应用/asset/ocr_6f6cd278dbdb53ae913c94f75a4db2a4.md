```
def handle_data(account):
    for s in account.universe:
        order(s,100)

# 每只股票买至持仓 1 手 (100 股)

def handle_data(account):
    for s in account.universe:
        order_to(s,100)

# 每只股票买入价值为虚拟账户当前总价值的 10 %

def handle_data(account):
    for s in account.universe:
        order_pct(s,0.1)

# 每只股票买入卖出至价值为虚拟账户当前总价值的 10 %

def handle_data(account):
    for s in account.universe:
        order_pct_to(s,0.1)
```

在此基础上，可以做一些条件判断。例如，设定一个最简单的买入卖出条件，当股票价格低于 4 的时候买入，当股票价格涨幅达到 1.25 倍的时候卖出。

```
def handle_data(account):
    for stock in account.universe:
        # 股票来自股票池，并且优矿平台自动剔除了当天停牌退市的股票
        p = account.referencePrice(stock)    # 股票前一天的收盘价
        cost = account.valid_seccost.get(stock)  # 股票的平均持仓成本
        if 0 < p < 4 and not cost:
            # 判断股票价格低于 4，并且当前没有买入该股票
            order_pct_to(stock, 0.1)
            # 将满足条件的股票买入，总价值占虚拟账户的 10 %
        elif cost and p >= cost * 1.25:
            # 卖出条件，价格 p 涨到买入价的 1.25 倍
            order_to(stock, 0)    # 将满足条件的股票卖到剩余 0 股，即全部卖出
```

# 17.7 组合成完整的量化策略

把上面的几点组合起来，就可以生成一个最简单的量化策略了。

```
# 从沪深 300 指数中任意挑选一只股票，涨到 1.25 倍后卖出

start = '2014 - 01 - 01'    # 回测的起止时间是 2014 年 1 月 1 日至 2015 年 6 月 1 日
end = '2015 - 06 - 01'

benchmark = 'HS300'    # 参照标准为沪深 300 指数的走势
universe = set_universe('HS300')    # 股票池为沪深 300 指数的成分股
capital_base = 100000    # 初始资金为 10 万元

def initialize(account):
    # 初始化一个全新的账户
    pass

def handle_data(account):
    for stock in account.universe:
        p = account.reference_price(stock)
        cost = account.security_cost.get(stock)
        if 0 < p < 4 and not cost:
            order_pct_to(stock, 0.1)
        elif cost and p >= cost * 1.25:
```