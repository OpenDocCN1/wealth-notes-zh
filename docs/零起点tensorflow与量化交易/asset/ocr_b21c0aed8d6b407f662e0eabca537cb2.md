# 案例 3-9：TQ 版本日线数据

案例 3-9 文件名为 kc309_tqday.py，主要介绍如何使用 TQ_bar 类、zData 金融数据集，绘制对应的日线 K 线图。

下面我们介绍主流程代码。第 1 组代码如下：

```
#1 预处理
pd.set_option('display.width', 450)
pyplt=py.offline.plot
```

按照惯例，第 1 组代码都是设置数据、变量以及程序的运行环境。

第 2 组代码如下，主要是设置参数，读取输入数据。

```
#2 set_data,init
codLst=['000001']
rs0=zsSys.rdatCN0  #'/zDat/cn/'
print('\n#1,set_data,init,',rs0)
qx=ztq.tq_init(rs0,codLst)
```

初始化 TQ_bar 类变量 qx。请注意，codLst 不是单只股票代码，而是一个 List，这是因为 TopQuant 支持股票池，所以采用列表形式。

第 3 组代码如下，主要是根据 qx 变量，查看当前工作股票的基本数据。

```
#3
print('\n#3,qx.wrkStk')
xcod=qx.wrkStkCod
print('\nqx.wrkStkCod,',qx.wrkStkCod)
print('\nqx.wrkStkInfo,',qx.wrkStkInfo)
print('\nqx.wrkStkDat.tail')
print(qx.wrkStkDat.tail())
```

# 在 qx 变量中:

+   - qx.wrkStkCod 变量用于保存当前工作股票代码。
- qx.wrkStkDat 变量用于保存当前工作股票数据。

第 3 组代码对应的输出信息如下：

```
#3,qx.wrkStk
qx.wrkStkCod, 000001
```