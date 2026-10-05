```
print('\n#2,设置 BT 回测初始参数: 起始资金等')

dmoney0=100000.0
cerebro.Broker.setcash(dmoney0)
dcash0=cerebro.Broker.startingcash

#
print('\n\t#2-2,设置数据文件,需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fdat=rs0+xcod+'.csv'
print('\t@数据文件名: ',fdat)

#
print('\t 设置数据BT 回测运算: 起始时间、结束时间')
print('\t 数据文件,可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t 格式为: 标准OHLC 格式,可以是日线、<|message|>时数据')
t0str,t9str='2018-10-01','2018-12-31'
data=tq.pools_get4fn(fdat,t0str,t9str)

#
# 增加一个股票名称变量 xcod, 可以在策略函数、绘图中使用
cerebro.adddata(data,name=xcod)
```

在设置完量化分析数据源等基础数据后，添加策略和与策略相关的变量参数。

本案例中的策略添加代码，就在主程序模块中。

策略参数中的fgPrint输出打印标志变量，在策略调试阶段通常都设