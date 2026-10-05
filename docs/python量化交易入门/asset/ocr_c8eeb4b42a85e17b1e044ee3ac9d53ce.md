```
while True:
    t = datetime.datetime.now()

    # 每天到达任务下载时间后，执行数据下载的操作
    if t.time() > taskTime and (taskCompletedDate is None or t.date() != taskCompletedDate):
        # 下载1000条1分钟K线数据，足以覆盖两天的行情
        downloadAllMinuteBar(1000, symbols)

        # 更新任务完成的日期
        taskCompletedDate = t.date()
    else:
        print(u'当前时间%s, 任务定时%s' % (t, taskTime))

    time.sleep(60)
```

运行程序后实现的天勤终端自动定时下载如图 4-6 所示。

![](bbox=[0.17, 0.47, 0.75, 0.75])

图 4-6 天勤终端自动定时下载

# 4.2 K 线生成模块

在期货市场中，交易系统会实时收到交易所每秒 2 次的 Tick 行情推送，其行情信息有开盘价、最高价、最低价、最新价、成交量、成交额、买一价、买一量、