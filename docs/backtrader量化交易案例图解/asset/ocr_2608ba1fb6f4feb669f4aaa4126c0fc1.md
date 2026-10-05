# 8.4 Observers观测子模块

在BackTrader量化程序中，自定义买卖点符号的修改调用都是在主流程中通过Observers观测子模块完成的，代码如下：

```
bt.observers.BuySell = MyBuySell
```

Observers观测子模块类似于传统的Log日志模块或者后台监控模块，主要用于记录后台交易数据及图信息，如图8-8所示。

![](bbox=[0.122, 0.299, 0.873, 0.562])

图8-8 Observers观测子模块架构图

在程序中，通过调用Observers观测子模块修改BuySell参数。

BackTrader的设计结构非常简单，也非常灵活。传统的其他量化软件通常需要修改很多模块和很多环节的代码才能完成对细节的修改，而使用BackTrader量化软件对细节进行修改就相对比较简单。