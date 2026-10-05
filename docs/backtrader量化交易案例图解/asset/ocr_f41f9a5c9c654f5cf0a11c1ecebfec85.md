```
class TQSta001(bt.Strategy):
    #定义 MA 均线策略的周期参数变量，默认值是 15
    params = (
        ('maperiod', 15),
    )
```

策略变量参数采用的是dict字典格式，以方便调用。

Buy买单操作，设置的条件是当天收盘价低于MA均线，相关代码如下：

```
# 使用最简单的 SMA 均线策略
if self.dataclose[0] < self.sma[0]:
    # 如果当前收盘价<当前的 SMA 均价
    # MA 均线策略买入信号成立
    # BUY, BUY, BUY!!!买！买！买！
    # 使用默认参数交易：数量、佣金等
    self.log('设置买单 BUY CREATE, %.2f, name : %s' %
            (self.dataclose[0],self.datas[0]._name))
    # 采用 track 模式，设置 order 订单
    # 回避第二张订单 2nd order 连续交易问题
    self.order = self.buy()
```

卖单操作设置的条件是当天收盘价高于MA均线，相关代码如下：