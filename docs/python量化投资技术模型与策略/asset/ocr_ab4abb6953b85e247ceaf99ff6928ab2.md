```
# 若当前持有空头仓位，则先平多，再开空
elif self.pos > 0:
    self.sell(price, 1)
    self.short(price, 1)

self.put_event()

def on_order(self, order: OrderData):
    """
    通过该函数收到委托状态更新推送
    """
    pass

def on_trade(self, trade: TradeData):
    """
    通过该函数收到成交推送
    """
    # 成交后若策略逻辑仓位发生变化，则需要通知界面更新
    self.put_event()

def on_stop_order(self, stop_order: StopOrder):
    """
    通过该函数收到本地停止单推送
    """
    pass
```

在文件头部的一系列import中，最重要的就是CtaTemplate，这是我们开发CTA策略所用的策略模板基类，策略模板提供了一系列以on_开头的回调函数，用于接收事件推送，以及其他主动函数用于执行操作（委托、撤单、记录日志等）。

所有开发的策略类，都必须继承CtaTemplate基类，然后在需要的回调函数中实现策略逻辑，即当某件事情发生时，我们需要执行的对应操作：比如，当收到1分钟K线推送时，我们需要计算均线指标，然后判断是否要执行交易。