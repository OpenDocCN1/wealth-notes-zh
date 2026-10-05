# Python 量化交易

线的数量即可。

```
from dataService import *
if __name__ == '__main__':
    symbols = ["SHFE.cu1901"]
    downloadAllMinuteBar(1000, symbols)
```

运行时会调用在同一文件夹 dataService.py 的 downloadAllMinuteBar 函数进行下载，天勤终端下载成功界面如图 4-5 所示。

![](bbox=[0.268, 0.329, 0.784, 0.559])

# 3. 自动定时下载模块

自动定时下载模块在 runService.py 文件中，同理只要输入合约代码，如 CFFEX.IF1810，启动程序后定时在每个交易日 17 点的某一分钟（随机的任务下载时间）自动下载更新历史行情数据，1000 条 1 分钟 K 线数据足以覆盖两天的行情。

```
if __name__ == '__main__':
    taskCompletedDate = None

    # 生成一个随机的任务下载时间，用于避免所有用户在同一时间访问数据服务器
    taskTime = datetime.time(hour=17, minute=0)

    symbols = ["CFFEX.IF1810", "CFFEX.IF1811", "CFFEX.IF1812",
               "CFFEX.IH1810", "CFFEX.IH1811", "CFFEX.IH1812",
               "CFFEX.IC1810", "CFFEX.IC1811", "CFFEX.IC1812",]
    # 进入主循环
```