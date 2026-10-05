# 图11-5 Position子模块关系属性示意图

由图11-5可以看出，其主要相关的子模块库有LineBuffer、Order、Sizer、Trade等。

在Position仓位管理子模块中最常用是getposition函数。getposition函数用于读取当前仓位数据，其实是position内部变量的二次封装。

此外，使用BackTrader的position类函数还可以看到买入时的价格，随时根据平均仓位成本进行操作。

下面再看看案例策略源码，position函数主要用于next函数：