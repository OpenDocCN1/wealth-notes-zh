# 图8-11 程序生成的plot图

由图8-11可以看到，买卖符号发生了变化。在以往的案例中都是采用三角符号表示买入和卖出的，但在图8-11中使用对勾符号（√）表示买入，使用叉符号（×）表示卖出，同时这两个符号的尺寸也更大一些。

相关操作的代码如下：

```
class MyBuySell(bt.observers.BuySell):
    plotlines = dict(
        #buy=dict(marker='$\u21E7$', markersize=12.0),  #arrow
        #sell=dict(marker='$\u21E9$', markersize=12.0)
        #
        #buy=dict(marker='$++$', markersize=12.0),
        #sell=dict(marker='$--$', markersize=12.0)
        #
        buy=dict(marker='$√$', markersize=12.0),
        sell=dict(marker='$X$', markersize=12.0)
    )
```

源码中的买卖点三角符号需要在class定义MyBuySign代码部分进行修改。markersize参数表示买卖点符号的大小，在本案例中是12。

注意，在源码中，“√”和“×”符号需要用两个“$”符号表示，并括住符号代码，因为需要使用UTF代码符号：

```
buy=dict(marker='$√$', markersize=12.0),
sell=dict(marker='$X$', markersize=12.0)
```

在源码中，预设了几组买卖点符号，大家可以自行修改。首先使用第一组预定义买卖点符号：

```
buy=dict(marker='$\u21E7$', markersize=12.0),  #arrow
sell=dict(marker='$\u21E9$', markersize=12.0)
```