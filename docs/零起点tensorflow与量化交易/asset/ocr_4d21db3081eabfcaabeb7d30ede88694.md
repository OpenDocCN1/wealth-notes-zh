# ## 案例 3-12：个股和指数曲线对照图

在进行实盘分析时，经常使用个股和指数曲线的对照图。

案例 3-12 文件名为 kc12\_txday2.py，主要介绍同时使用个股、指数数据，绘制对应的 K 线对比曲线图。

因为本案例将介绍两种不同的绘图模式，所以代码较长，下面分组进行介绍。第 1 组代码如下：

```
```
#1 预处理
pd.set_option('display.width', 450)
pyplt=py.offline.pyplot
```
```

按照惯例，第 1 组代码都是设置数据、变量以及程序的运行环境。第 2 组代码如下，主要是设置参数，读取输入数据。

```
```
#2
#2 set_data,init
codLst=['000001','000002']
inxLst=['000001','000002']
rs0=zsys.rdatCN0   #'\zDat\cn'
print('\n#2, set_data,init,',rs0)
qx=ztq.tq_init(rs0,codLst,inxLst)
```
```

调用 tq\_init 初始化函数，初始化量化全局变量 qx。第 3 组代码有两段，第 1 段代码如下：

```
```
#3.A
print('\n#3.A,qx.wrKStk')
xcod=qx.wrKStkCod
print('\nqx.wrKStkCod,',qx.wrKStkCod)
print('\nqx.wrKStkInfo,',qx.wrKStkInfo)
```
```

tq\_init 初始化函数运行后，读取当前的 qx.wrKStkDat 股票数据，对应的输出信息如下：

```
```
#3.A,qx.wrKStk
qx.wrKStkCod, 000001
qx.wrKStkInfo, code  name  ename   id industry id_industry area id_area
1046       000001 平安银行 PABH_000001 1 银行     25     深圳 264
```
```