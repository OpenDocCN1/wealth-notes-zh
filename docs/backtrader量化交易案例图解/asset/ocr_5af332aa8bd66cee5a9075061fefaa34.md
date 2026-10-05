```
print('\n#1,设置 BT 量化回测程序入口')
cerebro = bt.Cerebro()

#
print('\n#2,设置 BT 回测初始参数: 起始资金等')
dmoney0=100000.0
cerebro.broker.setcash(dmoney0)
dcash0=cerebro.broker.startingcash

#
#
print('\n\t#2-2,设置数据文件, 需要按时间字段正序排序')
rs0='data/stk/'
xcod='002046'
fd...
```