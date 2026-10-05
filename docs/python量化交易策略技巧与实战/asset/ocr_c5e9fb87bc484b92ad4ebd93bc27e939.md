```
elif K1<=80 and K1<D1 and context.portfolio.positions[security].closeable_amount > 0:
```

# 14.4.4 编写收盘后运行函数

收盘后运行函数实现代码如下：

```
def after_market_close(context):
    log.info(str('函数运行时间(after_market_close):'+str(context.current_dt.time())))
    #得到当天所有成交记录
    trades = get_trades()
    for _trade in trades.values():
        log.info('成交记录: '+str(_trade))
    log.info('一天的交易结束，祝你心情愉快！')
```

# 14.4.5 KD 指标量化交易策略的回测

在回测之前，设置开始时间为 2016 年 6 月 1 日，结束时间为 2018 年 6 月 1 日，设置账户交易资金为 100 000 元，交易频率为每天，如图 14.10 所示。

![](bbox=[0.285, 0.505, 0.68, 0.57])

设置好各项参数后，单击“编译运行”按钮，即可编译运行 KD 指标量化交易策略，运行完毕后，就可以看到策略收益和基准收益对比图表，如图 14.11 所示。

![](bbox=[0.145, 0.665, 0.825, 0.895])