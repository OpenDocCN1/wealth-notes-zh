# 从零开始学 Python 大数据与量化交易

按价值下单函数，如果创建订单成功，则返回 Order 对象，若失败则返回 None。按价值下单函数的实例代码如下：

```
#卖出价值为10000元的平安银行股票
order_value('000001.XSHE', -10000)
#买入价值为10000元的平安银行股票
order_value('000001.XSHE', 10000)
```

# 10.4.4 目标价值下单函数

目标价值下单函数的语法格式如下：

```
order_target_value(security, value, style=None, side='long', pindex=0)
```

目标价值下单函数与按股数下单函数参数几乎一样，只有第二个参数不一样。第二个参数 value 表示期望的股票最终价值。

目标价值下单函数，如果创建订单成功，则返回 Order 对象，若失败则返回 None。目标价值下单函数的实例代码如下：

```
#卖出平安银行所有股票
order_target_value('000001.XSHE', 0)
#调整平安银行股票仓位到10000元价值
order_target_value('000001.XSHE', 10000)
```

# 10.4.5 撤单函数

撤单函数的语法格式如下：

```
cancel_order(order)
```

参数 order 是指 Order 对象或者 order_id。撤单函数，如果取消委托成功，则返回 Order 对象，委托不存在则返回 None。撤单函数的实例代码如下：

```
#自定义每个交易日结束运行函数
def after_trading_end(context):
    # 得到当前未完成订单
    orders = get_open_orders()
    # 利用 for 循环撤销订单
    for _order in orders.values():
        cancel_order(_order)
```

# 10.4.6 获取未完成订单函数

获取未完成订单函数的语法格式如下：