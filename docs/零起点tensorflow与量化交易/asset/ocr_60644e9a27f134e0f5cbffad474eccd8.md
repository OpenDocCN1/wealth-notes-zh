第 10 章 Tick 数据回溯分析

![](bbox=[0.23, 0.1, 0.77, 0.51])

第 1 组代码如下：

```
#1 预处理
pd.set_option('display.width', 450)
pypI=py.offline.plot
```

预处理，设置运行环境参数。

第 2 组代码如下：

```
#2 set_data
xlst=['agg', 'hyg', 'spy', 'eem', 'efa']
fdat='data/bt05.csv'
print('\\n#2, xlst,', xlst)
```

设置数据。本案例中使用的是美股数据，xlst 是股票池中美股的股票代码。其对应的输出信息如下：

```
#2, xlst, ['agg', 'hyg', 'spy', 'eem', 'efa']
```