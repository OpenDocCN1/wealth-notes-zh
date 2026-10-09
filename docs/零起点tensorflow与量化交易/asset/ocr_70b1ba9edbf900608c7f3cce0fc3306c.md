# 案例 8-9：LSTM 量化分析程序

案例 8-9 文件名为 kc809_lstm_all.py，是一个完整的案例程序。因为采用 All-in-one 模式，代码较长，所以我们把全部程序分为十多个小组进行讲解。

# 8.3.1 数据整理

首先介绍数据整理环节的程序。第 1 组代码如下：

```
#1 预处理
pd.set_option('display.width', 450)
pd.set_option('display.float_format', zt.xfloat3)
pyplt=py.offline.plot
```

预处理，设置运行环境参数。

第 2 组代码如下：

```
#---------step #1,data pre
#2 set data
print('\n#2,set data')
inxLst=['000001']
codLst=['000001','002046','600663','000792','600029','000800']
xlst=['inx','total']+codLst
tim0Str,tim9Str='2017-01-01','2017-09-30'
rs0=zsys.rdatCN0        #'/zDat/cn/'
prjNam,ks gn='LSTM9/','avg'
ftg0='tmp/'+prjNam
print('rs0',rs0)
```

设置基本参数，主要用于回溯初始化函数 bt_init。其中各参数的含义请见案例 8-6。

第 3 组代码如下：

```
#3 init.qx
print('\n#3,init.qx')
qx=zbt.bt_init(rs0,codLst,inxLst,tim0Str,tim9Str,ksgn,prjNam)
```