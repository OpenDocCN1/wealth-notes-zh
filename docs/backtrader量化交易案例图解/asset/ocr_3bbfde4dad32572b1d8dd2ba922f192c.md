```
# 创建一个自定义策略类 class。
class TQSta001(bt.Strategy):

    def log(self, txt, dt=None):
        # log 记录函数。
        dt = dt or self.datas[0].datetime.date(0)
        print('%s, %s' % (dt.isoformat(), txt))

    def __init__(self):
        # 默认数据,一般使用股票池当中下标为 0 的股票,
        # 通常使用收盘价作为主要分析数据字段。
        self.dataclose = self.datas[0].close

    def next(self):
        # next 函数是最重要的交易(运算分析)函数,
        # 调用 log 函数, 输出 BT 回测过程当中,
        #工作节点数据包 BAR, 对应的收盘价。
        self.log('Close 收盘价, %.2f' % self.dataclose[0])

#-------------
print('\n#1,设置 BT 量化回测程序入口')
cerebro = bt.Cerebro()

#
print('\n#2-1,设置 BT 回测初始参数: 起始资金等')
```