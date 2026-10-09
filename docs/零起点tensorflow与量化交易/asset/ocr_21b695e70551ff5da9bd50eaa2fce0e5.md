# 第 8 章 LSTM 与量化分析

```
```
qx=zbT.bt_init(rsO, codLst, inxLst, timOStr, tim9Str, ksgn, prjNam)
zTg.TQ_prVar(qx)
```
```

根据所设置的参数，初始化回溯测试的全局变量 qx。

TopQuant 极宽智能量化系统采用的是单变量模式，这样便于简化维护系统各个模块之间的接口，通常只需传递一个参数变量 qx 即可。

但缺点是，qx 是一个宏变量，保存了完整的股票池数据，内存占用很大，而且需要一个额外的 class（类）定义。

qx 变量的初始化，其实是集成在 zTools_tQ 模块的 tQ_init 函数中的。在 zTools_bT 极宽回溯模块的 bT_init 函数内部调用了 tQ_init 函数，生成 qx 变量。运行 bT_init 函数，会生成以下信息：

```
```
#3,init.qx
tQ_init name...
tQ_init pools...

clst: ['000001', '002046', '600663', '000792', '600029', '000800']
1 / 6 / zData/cn/day/000001.csv
2 / 6 / zData/cn/day/002046.csv
3 / 6 / zData/cn/day/600663.csv
4 / 6 / zData/cn/day/000792.csv
5 / 6 / zData/cn/day/600029.csv
6 / 6 / zData/cn/day/000800.csv
clst: ['000001']
1 / 1 / zData/cn/xday/000001.csv
tQ_init work data...
```
```

以上信息表示根据股票池中的股票代码，逐一加载数据。为了提高计算性能，作者采用了全内存计算模式，尽量把所有数据一次性加载到内存中。第 3 组代码运行结束后，调用 tQ_prVar 函数，输出 qx 变量所保存的主要数据。

```
```
zTg.TQ_prVar(qx)
```
```

如图 8-25 所示是 qx 变量所保存的内部数据信息（部分）。