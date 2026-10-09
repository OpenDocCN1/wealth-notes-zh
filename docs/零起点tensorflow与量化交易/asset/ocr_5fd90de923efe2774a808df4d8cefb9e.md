# 8.2.3 回溯分析

回溯分析，其英文名称是 **BackTest**。具体到金融股市的量化回溯分析，就是使用过去的历史数据，对投资策略进行测试，以检验投资策略是否有效；或者通过参数优化，调整投资策略的细节。

# 案例 8-7: 回溯分析

案例 8-7 文件名为 kc807_bt.py，本案例主要介绍回溯分析的具体内容。下面我们分组进行讲解。

第 1 组代码如下：

```
#1 预处理
pd.set_option('display.width', 450)
pyplt=py.offline.plot
```

预处理，设置运行环境参数。

# 1. 读取数据

第 2 组代码如下：

```
#2 rd.var
print('\n#2,rd.var&model')
fmxD='data/TM_'; qx=ztq.ai_varRd(fmxD);
```

我们将前面在数据整理环节生成的数据文件复制并保存在 data 数据目录下。其中：

+   - data/TM_tqvar.pkl 是 qx 变量参数文件。
- data/TM_lstm010.mx 是案例中使用的神经网络模型文件。

如图 8-31 所示是读取数据文件后，qx 变量内部参数信息（部分）。

# 2. 调整回溯参数

第 3 组代码如下：

```
#3
print('\n#3 set.bt.var')
qx.staFun=zsta. lstm010
```