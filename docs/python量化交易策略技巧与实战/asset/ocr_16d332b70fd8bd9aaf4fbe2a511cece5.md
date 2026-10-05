```
# 记录这次卖出
log.info("卖出股票 %s" % (security))
```

需要注意该代码通过调用 MACD 函数，获得股票的 MACD 指标的 DIF、DEA 和 MACD 的值；还需要注意该代码中的参数 check_date=context.current_dt，表示获取当前回测日期。

在此，买入股票的条件是如果当前有余额，并且 DIFF、DEA 均为正，DIFF 向上突破 DEA，具体代码如下：

```
if macd_diff > 0 and macd_dea >0 and macd_diff > macd_dea :
```

卖出股票的条件是如果 DIFF、DEA 均为负，DIFF 向下跌破 DEA，并且目前有头寸，具体代码如下：

```
elif macd_diff < 0 and macd_dea < 0 and macd_diff < macd_dea and context.portfolio.positions[security].closeable_amount > 0:
```

# 14.3.3 MACD 指标量化交易策略的回测

在回测之前，先设置开始时间为 2017 年 6 月 1 日，结束时间为 2018 年 5 月 4 日，账户交易资金为 100 000 元，交易频率为每天，如图 14.8 所示。

![](bbox=[0.335, 0.493, 0.706, 0.521])

图 14.8 设置回测时间和频率

设置好各项参数后，单击“编译运行”按钮，即可编译运行 MACD 指标量化交易策略，运行完毕后，即可看到策略收益和基准收益对比图表，如图 14.9 所示。

![](bbox=[0.14, 0.626, 0.901, 0.887])

图 14.9 策略收益和基准收益对比图表