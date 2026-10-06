# 从零开始学 Python 大数据与量化交易

```
# 用所有 cash 买入股票
order_value(security, cash)
# 如果上一时间点价格低于 5 天平均价, 则空仓卖出
elif ma_n1 > ma_n2 and ma_n2> ma_n3 and current_price > upperband and context.portfolio.positions[security].closeable_amount > 0:
    # 记录这次卖出
    log.info("收盘价站上 BOLL 指标上轨, 卖出 %s" % (security))
    # 卖出所有股票,使这只股票的最终持有量为 0
    order_target(security, 0)
```

在这里买入股票的条件: 当 5 日均线大于 10 日均线, 10 日均线大于 30 日均线, 同时, 收盘价格高于 BOLL 的中轨, 具体代码如下:

```
if ma_n1 > ma_n2 and ma_n2> ma_n3 and current_price > middleband :
```

卖出股票的条件: 当 5 日均线小于 10 日均线, 10 日均线小于 30 日均线, 同时, 收盘价格高于 BOLL 的上轨, 并且有头寸, 具体代码如下:

```
elif ma_n1 > ma_n2 and ma_n2> ma_n3 and current_price > upperband and context.portfolio.positions[security].closeable_amount > 0:
```

# 16.5.4 编写收盘后运行函数

收盘后运行函数的实现代码如下:

```
## 收盘后运行函数
def after_market_close(context):
    log.info(str('函数运行时间(after_market_close):'+str(context.current_dt.time())))
    #得到当天所有成交记录
    trades = get_trades()
    for _trade in trades.values():
        log.info('成交记录: '+str(_trade))
    log.info('一天结束')
    log.info('##############################################################')
```

# 16.5.5 BOLL 指标量化交易策略的回测

在回测之前, 设置开始时间为 2016 年 6 月 1 日, 结束时间为 2018 年 6 月 1 日。设置账户交易资金为 10 万元, 交易频率为每天, 如图 16.12 所示。

![](bbox=[0.275, 0.82, 0.72, 0.88])

图 16.12 设置回测时间和频率