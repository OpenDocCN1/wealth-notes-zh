```
print('\n\t#2-2,设置数据文件,需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fd at=rs0+xcod+'.csv'
print('\t@数据文件名: ',fd at)

#
print('\t设置数据BT回测运算:起始时间、结束时间')
print('\t数据文件,可以是股票期货、外汇黄金、数字货币等交易数据')
print('\t格式为:标准OHLC格式,可以是日线、分钟数据')
#t0str,t9str='2015-01-01','2018-12-31'
t0str,t9str='2018-01-01','2018-12-31'
data=tq.pools_get4fn(fd at,t0str,t9str)

#
#增加一个股票名称变量x cod,可以在策略函数、绘图中使用
cerebro.adddata(data,name=x cod)
#
#
print('\n\t#2-3,添加BT量化回测程序对应的策略参数')
print('\n\t# 案例中使用的是MA均线策略')
cerebro.addstrategy(TQSta001)
#
print('\n\t#2-4,添加经纪人佣金,默认为:千一')
cerebro.broker.setcommission(commission=0.001)
#
print('\n\t#2-5,设置每手交易数目为:10,不再使用默认值:1手')
cerebro.addsizer(bt.sizers.FixedSize, stake=10)

print('\n\t#2-5,设置addanalyzer分析参数')

#
cerebro.addanalyzer(SQN,_name='sqnAnz')
#
cerebro.addanalyzer(bt.analyzers.SharpeRatio, _name = 'SharpeRatio', legacyannual=True)
```