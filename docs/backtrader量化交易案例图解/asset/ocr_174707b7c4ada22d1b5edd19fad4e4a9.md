买卖点就是买卖时机。

下面看一看买卖点的设置。在策略的next函数源码中，与买卖点设置相关的代码如下：

```
def next(self):
...
if not self.position:
    if self.dataclose[0] < self.sma[0]:
        self.log('设置买单 BUY CREATE, %.2f, name : %s'
        % (self.dataclose[0],self.datas[0]._name))
        self.order = self.buy()
else:
    if self.dataclose[0] > self.sma[0]:
        self.log('设置卖单 SELL CREATE, %.2f, name : %s'
        % (self.dataclose[0],self.datas[0]._name))
        self.order = self.sell()
```

将当天收盘价与MA均线价格进行比较，再分别进行买单操作和卖单操作。

买卖点的执行就是买单和卖单的执行，它们是在量化程序后台自动运行的。当市场出现和订单匹配的价格时，订单会自动执行并通过订单管理函数中的notify_order订单通知函数生成输出信息。

以下是notify_order订单通知函数中与订单执行相关的主要代码：