# 13.4 SignalStrategy信号策略类

信号函数定义的代码，在init初始化函数中使用最简单的MA均线指标，与当天收盘价进行对比，生成信号函数，还增加了一个自定义信号策略类sgn_st001， sgn_st001源自SignalStrategy信号策略类。

如图13-11所示是BackTrader量化软件中SignalStrategy信号策略类模块关系属性示意图。

![](bbox=[0.13, 0.286, 0.866, 0.567])

图13-11 SignalStrategy信号策略类模块关系属性示意图

由图13-11可以看出， SignalStrategy信号策略类隶属于Strategy类，其主要相关子模块库有Analyzer、Order等。

案例相关程序代码如下：