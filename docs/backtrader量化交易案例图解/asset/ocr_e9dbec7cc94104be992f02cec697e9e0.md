```
dmmoney0=100000.0
cerebro.broker.setcash(dmoney0)
dcash0=cerebro.broker.startingcash

#
#
print('\n\t#2-2,设置数据文件, 需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fdat=rs0+xcod+'.csv'
print('\t@数据文件名: ',fdat)

#
print('\t设置数据BT回测运算: 起始时间、结束时间')
print('\t数据文件, 可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t格式为: 标准OHLC格式, 可以是日线、分时数据')
#t0str,t9str='2010-01-01','2018-12-31'
t0str,t9str='2018-10-01','2018-12-31'
#t0str,t9str='',''
data=tq.pools_get4fn(fdat,t0str,t9str)
#
#----ha-k
data.addfilter(bt.filters.HeikinAshi)

# 增加一个股票名称变量xcod, 可以在策略函数内部使用
cerebro.adddata(data,name=xcod)
#
#
print('\n\t#2-3,添加BT量化回测程序对应的策略参数')
print('\n\t# 案例中使用的MA均线策略')
cerebro.addstrategy(TQSta001)
#
print('\n\t#2-4,添加Broker经纪人佣金, 默认: 千一')
```