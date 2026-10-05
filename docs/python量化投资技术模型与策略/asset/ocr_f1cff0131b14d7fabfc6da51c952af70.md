再之后就是根据信号进行的交易逻辑了。这里需要注意的是，pos变量是由引擎维护的。但是经过笔者的测试，必须要在on_trade()函数里加上self.put_event()才能保证pos更新的及时和准确性。具体代码如下：

```
if bar.datetime.time() < self.exitTime:

#没有仓位
if self.pos == 0:
    if bar.close > self.dayOpen:
        if not self.longEntered:
            vtOrderID = self.buy(self.longEntry, self.fixedSize, stop=True)
            self.orderList.append(vtOrderID)
    else:
        if not self.shortEntered:
            vtOrderID = self.short(self.shortEntry, self.fixedSize, stop=True)
            self.orderList.append(vtOrderID)

# 持有多头仓位
elif self.pos > 0:
    self.longEntered = True

    # 多头止损单
    vtOrderID = self.sell(self.shortEntry, self.fixedSize, stop=True)
    self.orderList.append(vtOrderID)
    # 空头开仓单
    if not self.shortEntered:
        vtOrderID = self.short(self.shortEntry, self.fixedSize, stop=True)
        self.orderList.append(vtOrderID)
# 持有空头仓位
elif self.pos < 0:
    self.shortEntered = True

    # 空头止损单
    vtOrderID = self.cover(self.longEntry, self.fixedSize, stop=True)
    self.orderList.append(vtOrderID)
```