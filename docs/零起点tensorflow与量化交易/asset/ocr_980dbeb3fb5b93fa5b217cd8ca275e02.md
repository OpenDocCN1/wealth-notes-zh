```
ztq.tq_prVar(qx)
```

根据所设置的参数，初始化回溯测试的全局变量 qx。

运行 bt_init 函数，会生成如下信息。

```
```text
#3,init.qx
tq init name...
tq init pools...

clist: ['000001', '002046', '600663', '000792', '600029', '000800']
1 / 6 / zDat/cn/day/000001.csv
2 / 6 / zDat/cn/day/002046.csv
3 / 6 / zDat/cn/day/600663.csv
4 / 6 / zDat/cn/day/000792.csv
5 / 6 / zDat/cn/day/600029.csv
6 / 6 / zDat/cn/day/000800.csv

clist: ['000001']
1 / 1 / zDat/cn/xday/000001.csv
tq init work data...
```
```

如上信息表示根据股票池中的股票代码，逐一加载数据。为了提高计算性能，笔者采用了全内存计算模式，尽量把所有数据一次性加载到内存中。

第 3 组代码运行完成后，调用 tq_prVar 函数，输出 qx 变量所保存的主要数据。

```
ztq.tq_prVar(qx)
```

如图 8-43 所示是部分输出信息。

第 4 组代码如下：

```
```python
#4 set.bT.var
print('\n#4,set.BT.var')
qx.preFun=zsta.avg01_dpre
qx.preVars=[10]
qx.staFun=zsta.avg01
qx.staVars=[1,0,1,2]
```
```