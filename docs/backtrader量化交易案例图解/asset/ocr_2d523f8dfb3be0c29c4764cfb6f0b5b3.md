由图5-9可以看出，其主要相关的子模块库有： Analyzer、 Strategy、 Transactions等。

下面看一看notify_order交易订单执行状态检查函数代码：

```
def notify_order(self, order):
    if order.status in [order.Submitted, order.Accepted]:
        # 检查订单执行状态 order.status:
        # Buy/Sell order submitted/accepted to/by Broker
        # Broker 经纪人: submitted 提交/accepted 接受, Buy 买单/Sell 卖单
        # 正常流程, 无须额外操作
```