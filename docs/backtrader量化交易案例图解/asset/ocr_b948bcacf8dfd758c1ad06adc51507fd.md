# 11.2 Cerebro类模块

在第11.1节，关于量化交易四个步骤中的第一步骤就是设置一个回测主函数：

```
print('\n#1,设置 BT 量化回测程序入口')

cerebro = bt.Cerebro()
```

cerebro在西班牙语中是“大脑”的意思，所以变量cerebro是BackTrader量化分析程序的全局主变量，表示量化引擎源自BackTrader的Cerebro模块库。

图11-2所示是Cerebro类的关系属性图。

![](bbox=[0.12, 0.37, 0.86, 0.81])

图11-2 Cerebro类的关系属性图