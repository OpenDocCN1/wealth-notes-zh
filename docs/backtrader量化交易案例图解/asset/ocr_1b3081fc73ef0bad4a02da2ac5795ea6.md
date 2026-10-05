# 14.3 BackTrader内置优化函数

BackTrader网站内置的optstrategy（简称为opt）参数优化函数，使用相对更加简单，但没有TopQuant极宽优化版本灵活。TopQuant极宽优化版本可以批量传递多组参数，甚至可以传递不同的策略。

如图14-1所示是BackTrader量化软件中，optstrategy参数优化函数关系属性示意图。

![](bbox=[0.118, 0.288, 0.845, 0.564])

图14-1 optstrategy参数优化函数关系属性示意图

由图14-1可以看出，该函数属于cerebro量化回测类的类函数之一。

在BackTrader量化软件中，通常采用addstrategy函数和以下代码添加策略：

```
cerebro.addstrategy(StrategyClass, *args, **kwargs)
```

其中，“*args, **kwargs”代表对应的策略参数，这是Python的动态参数模式。

在对策略进行优化时，使用optstrategy策略优化函数：

```
cerebro.optstrategy(StrategyClass, *args, **kwargs)
```