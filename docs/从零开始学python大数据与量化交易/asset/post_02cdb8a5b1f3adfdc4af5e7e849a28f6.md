
# 从零开始学 Python 大数据与量化交易

固定值：是指这个价差可以是一个固定的值(如 0.02 元，交易时加减 0.01 元)，设定方式为：FixedSlippage(0.02)。

百分比：是指这个价差可以是当时价格的一个百分比(如 0.2%，交易时加减当时价格的 0.1%)，设定方式为：PriceRelatedSlippage(0.002)。

设置滑点函数的实例代码如下：

```
# 设定滑点为固定值
set_slippage(FixedSlippage(0.02))
# 设定滑点为百分比
set_slippage(PriceRelatedSlippage(0.002))
```

> 提醒 如果没有调用 set_slippage 函数，系统默认的滑点是 PriceRelatedSlippage(0.00246)。

## 10.2.4 设置动态复权(真实价格)模式函数

设置动态复权(真实价格)模式函数的语法格式如下：

```
set_option('use_real_price', value)
```

注意：该函数只能在初始化函数（initialize）中调用。参数 value 的值，是 True 或 False。

设置动态复权(真实价格)模式函数的实例代码如下：

```
# 开启动态复权模式
set_option('use_real_price', True)
# 关闭动态复权模式
set_option('use_real_price',False)
```

> 提醒 为了更好地模拟，建议设置动态复权(真实价格)模式为 True。在对接实盘交易时，此选项会强制设成 True。

## 10.2.5 设置成交量比例函数

设置成交量比例函数的语法格式如下：

```
set_option('order_volume_ratio', value)
```

参数 value 是一个 float 值，根据实际行情限制每个订单的成交量。对于每一笔订单，如果是市价单，成交量不超过：每日成交量×value；如果是限价单，限价单撮合时设定分价表中每一个价格的成交量的比例。

设置成交量比例函数的实例代码如下：

```
# 设定成交量比例
set_option('order_volume_ratio', 0.25)  # 成交量不超过总成交量的 1/4
```

## 10.2.6 设置是否开启盘口撮合模式函数

设置是否开启盘口撮合模式函数的语法格式如下：

```
set_option('match_with_order_book', value)
```

设置是否开启盘口撮合模式只对模拟盘生效，默认开启，即 value 的值为 True。如果 value 的值为 False，关闭盘口撮合模式，使用 Bar 进行撮合。

设置是否开启盘口撮合模式函数代码如下：

```
#盘口撮合模式
set_option('match_with_order_book', True)
#盘口使用 Bar 进行撮合
set_option('match_with_order_book', False)
```

## 10.2.7 设置要操作的股票池函数

设置要操作的股票池函数的语法格式如下：

```
set_universe(security_list)
```

该函数只用于设定 history 函数的默认值 security_list，此外并无其他用处。参数 security_list 是指股票列表。

设置要操作的股票池函数代码如下：

```
set_universe(['000001.XSHE', '600000.XSHG'])
```

## 10.3 定时函数

在股票量化交易策略的初始化函数中，进行一些初始化设置后，最后运行了 3 个定时函数。下面就来具体讲解定时函数。

## 10.3.1 定时函数的定义及分类

定时函数是指在回测和模拟交易中指定每月、每周或者每天要运行的函数。定时函数可以在具体每月或每周的第几个交易日(或者倒数第几天)的某一分钟执行。

定时函数可以分为 3 种，分别是每日定时函数（run_daily）、每周定时函数（run_weekly）、每月定时函数（run_monthly）。

定时函数的语法格式如下：

```
# 按月运行
run_monthly(func, monthday, time='open', reference_security)
# 按周运行
run_weekly(func, weekday, time='open', reference_security)
# 每天内何时运行
run_daily(func, time='open', reference_security)
```

需要注意的是，定时函数在日级模拟中使用时，如果设置 time="open" 或 time="9:30"，那么股票量化交易策略的实际运行时间在 9:27—9:30 之间。股票量化交易策略类获取到的逻辑时间(context.current_dt)仍然是 9:30。

## 10.3.2 定时函数各项参数的意义

func: 是一个函数，并且该函数必须接受 context 参数。

monthday: 用来指定每月的第几个交易日，可以是负数，表示倒数第几个交易日。如果超出每月总交易日个数，则取临近的交易日执行。

weekday: 用来指定每周的第几个交易日，可以是负数，表示倒数第几个交易日。如果超出每周总交易日个数，则取临近的交易日执行。

time: 一个字符串，可以是具体执行时间，支持 time 表达式。比如"10:00", "01:00"或者"every_bar""open""before_open""after_close""close""morning" 和 "night"。

+   （1）every_bar: 只能在 run_daily 中调用；按天会在每天的开盘时调用一次，按分钟会在每天的每分钟运行。
（2）open: 开盘时运行（等同于"9:30"）。
（3）before_open: 早上 9:00 运行。
（4）after_close: 下午 15:30 运行。
（5）close: 下午 15:00 运行。
（6）morning: 早上 8:00 运行。
（7）night: 晚上 20:00 运行。

reference_security: 表示时间的参照标的。如果参照 000001.XSHG，交易时间为 9:30—15:00；如果参照 IF1812.CCFX，2016-01-01 之后的交易时间为 9:30—15:00，在此之前为 9:15—15:15；如果参照 A99910.XDCE，因为有夜盘，因此开始时间为 21:00，结束时间为 15:00。

> 提醒 由于量化交易策略可以应用于股票、股指期货、商品期货，并且商品期货有一些品种有夜盘，这样交易开始时间和结束时间就不一致，所以参数 time 和参数 reference_security 要根据具体交易品种来定。

## 10.3.3 定时函数的注意事项

定时函数的注意事项有 5 项，具体如下。

（1）参数 func 必须是一个全局的函数，不能是类的成员函数，实例代码如下：

```
def on_week_start(context):    #定义一个全局函数
    pass
class MyObject(object):        #定义一个类
    def on_week_start2(self, context):    #在类中定义一个成员函数
        pass
def initialize(context):        #量化交易策略的初始化函数
    run_weekly(on_week_start, 1)    #正常运行
    # 错误，下面的语句会报错
    run_weekly(MyObject().on_week_start2, 1)
```

（2）定时函数通过 history 或 attribute_history 获得每天数据时，是不包括当天的数据的。想要获得当天数据，只能按分钟来获取。

（3）定时函数可以重复调用，即初始化函数中可以有两个或多个同名定时函数，实例代码如下：

```
def on_week_start(context):
    pass
def on_week_end(context):
    pass
def initialize(context):        #初始化函数中有两个 run_weekly 定时函数
    #在每周的第一个交易日和最后一个交易日分别调用两个函数
    run_weekly(on_week_start, 1)
    run_weekly(on_week_end, -1)
```

（4）每次调用这些定时函数都会产生一个新的定时任务。如果想修改或者删除旧的定时任务，就要调用 unschedule_all（取消所有定时运行）来删除所有定时任务，然后再添加新的。

> 提醒 取消所有定时运行的代码是 unschedule_all()。

（5）如果定时函数在一月或一周交易日数不够，则 monthday 或 weekday 就无法满足。这时，可以找这周内最近的一个日期来执行。

## 10.3.4 定时函数的实例

首先定义 3 个全局函数，分别是 weekly()、monthly()、daily()，接着定义初始化函数，实现一次调用 monthly()、两次调用 weekly()、4 次调用 daily()，具体代码如下：

```
def weekly(context):
    print 'weekly %s %s' % (context.current_dt, context.current_dt.isoweekday())
def monthly(context):
    print 'monthly %s %s' % (context.current_dt, context.current_dt.month)
def daily(context):
    print 'daily %s' % context.current_dt
def initialize(context):
    # 指定每月第一个交易日，在开盘后 10 分钟执行，即 9:40
    run_monthly(monthly, 1, 'open+10m')
    # 指定每周倒数第一个交易日，在开盘前执行，即 9:00
    run_weekly(weekly, -1, 'before_open')
    # 指定每天收盘前 10 分钟运行，即 14: 50
    run_weekly(daily, 'close -10m')
    # 指定每天收盘后执行，即 15: 30
    run_daily(daily, 'after_close')
    # 指定在每天的 10:00 运行
    run_daily(daily, '10:00')
    # 指定在每天的 01:00 运行
    run_daily(daily, '01:00')
    # 参照股指期货的时间每分钟运行一次，必须选择分钟回测；否则每天执行
    run_daily(daily, 'every_bar', reference_security='IF1512.CCFX')
```

## 10.4 下单函数

在股票量化交易策略开盘时运行函数中用到了下单函数。下面就来具体讲解各种下单函数。

## 10.4.1 按股数下单函数

按股数下单函数的语法格式如下：

```
order(security, amount, style=None, side='long', pindex=0)
```

各项参数意义如下。

security: 标的代码。

amount: 交易数量，正数表示买入，负数表示卖出。

style: 下单类型有两种，分别是市价单（MarketOrder）和限价单（LimitOrder）。市价单是指不管什么价格均接下单，直到交易全部完成。限价单是指定一个价格，买入时不能高于它，卖出时不能低于它，如果不满足，则等待满足后再交易。该参数的默认值为市价单（MarketOrder），即 None 代表 MarketOrder。

side: 用来指定开多单还是空单。其中 long 代表开多单，short 代表开空单。默认为 long，即开多单。需要注意的是，股票和基金暂不支持开空单。

pindex: 在使用 set_subportfolios 创建多个仓位时，指定 subportfolio 的序号，从 0 开始。例如，0 指定第一个 subportfolio，1 指定第二个 subportfolio，默认为 0。

按股数下单函数，如果创建订单成功，则返回 Order 对象，若失败则返回 None。按股数下单函数的实例代码如下：

```
#买入平安银行股票 100 股
order('000001.XSHE', 100) # 下一个市价单
order('000001.XSHE', 100, MarketOrderStyle()) #下一个市价单，功能同上
order('000001.XSHE', 100, LimitOrderStyle(10.0)) # 以 10 元价格下一个限价单
```

## 10.4.2 目标股数下单函数

目标股数下单函数的语法格式如下：

```
order_target(security, amount, style=None, side='long', pindex=0)
```

目标股数下单函数与按股数下单函数的参数几乎一样，只有第二个参数不一样。第二个参数 amount 表示期望的最终数量。

目标股数下单函数，如果创建订单成功，则返回 Order 对象，若失败则返回 None。目标股数下单函数的实例代码如下：

```
# 卖出平安银行所有股票，即最终持有平安银行的股数为 0
order_target('000001.XSHE', 0)
# 买入平安银行所有股票到 100 股，即最终持有平安银行的股数为 100 股
order_target('000001.XSHE', 100)
```

## 10.4.3 按价值下单函数

按价值下单函数的语法格式如下：

```
order_value(security, value, style=None, side='long', pindex=0)
```

按价值下单函数与按股数下单函数的参数几乎一样，只有第二个参数不一样。第二个参数 value 表示股票价值。

按价值下单函数，如果创建订单成功，则返回 Order 对象，若失败则返回 None。按价值下单函数的实例代码如下：

```
#卖出价值为10000元的平安银行股票
order_value('000001.XSHE', -10000)
#买入价值为10000元的平安银行股票
order_value('000001.XSHE', 10000)
```

## 10.4.4 目标价值下单函数

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

## 10.4.5 撤单函数

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

## 10.4.6 获取未完成订单函数

获取未完成订单函数的语法格式如下：

```
get_open_orders()
```

该函数可以获得当天所有未完成的订单。获取未完成订单函数的实例代码如下：

```
#自定义每个交易日结束运行函数
def after_trading_end(context):
    #得到当前未完成订单
    orders = get_open_orders()
    for _order in orders.values():
        log.info(_order.order_id)
```

## 10.4.7 获取订单信息函数

获取订单信息函数的语法格式如下：

```
get_orders(order_id=None, security=None, status=None)
```

该函数可以获取当天的所有订单。
各项参数意义如下。
order_id: 订单 id。
security: 标的代码，可以用来查询指定标的的所有订单。
status: 表示订单状态，可以查询特定订单状态的所有订单。
获取订单信息函数的实例代码如下：

```
#自定义每个交易日结束运行函数
def after_trading_end(context):
    #得到当天所有订单
    orders = get_orders()
    for _order in orders.values():
        log.info(_order.order_id)
    # 根据订单 id 查询订单
    get_orders(order_id='1517627499')
    # 查询所有标的为 000002.XSHE 的订单
    get_orders(security='000002.XSHE')
    # 查询订单状态为 OrderStatus.held 的所有订单
    get_orders(status=OrderStatus.held)
    # 查询标的为 000002.XSHE 且状态为 OrderStatus.held 的所有订单
    get_orders(security='000002.XSHE', status=OrderStatus.held)
```

## 10.4.8 获取成交信息函数

获取成交信息函数的语法格式如下：
