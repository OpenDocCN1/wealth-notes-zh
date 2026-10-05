# 16.6.2  on_bar的逻辑

首先，我们需要维护一个报单列表orderList，调用on_bar的时候，最开始应撤销之前未成交的所有委托。这一 步很重要，否则报单的管理就可能会变得非常混乱，甚至导致出错。

```
# 撤销之前发出的尚未成交的委托（包括限价单和停止单）
for orderID in self.orderList:
    self.cancelOrder(orderID)
self.orderList = []
```

之后就是计算相应的指标值，根据指标值判断是否产生交易信号，代码如下：

```
if lastBar.datetime.date() != bar.datetime.date():
    # 如果已经初始化
    if self.dayHigh:
        self.range = self.dayHigh - self.dayLow
        self.longEntry = bar.open + self.k1 * self.range
        self.shortEntry = bar.open - self.k2 * self.range
    self.dayOpen = bar.open
    self.dayHigh = bar.high
    self.dayLow = bar.low
    self.longEntered = False
    self.shortEntered = False
else:
    self.dayHigh = max(self.dayHigh, bar.high)
    self.dayLow = min(self.dayLow, bar.low)
```