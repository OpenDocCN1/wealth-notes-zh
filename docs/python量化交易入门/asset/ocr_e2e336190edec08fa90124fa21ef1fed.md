```
elif self.pos > 0:
    self.intraTradeHigh = max(self.intraTradeHigh, bar.high)
    self.intraTradeLow = bar.low

    l = self.sell(self.intraTradeHigh*(1-self.trailingPrcnt / 100),
                abs(self.pos), True)
    self.orderList.extend(l)

# 持有空头仓位
elif self.pos < 0:
    self.intraTradeHigh = bar.high
    self.intraTradeLow = min(self.intraTradeLow, bar.low)

    l = self.cover(self.intraTradeLow*(1+self.trailingPrcnt / 100),
                abs(self.pos), True)
    self.orderList.extend(l)

# 同步数据到数据库
self.saveSyncData()

# 发出状态更新事件
self.putEvent()

#------------------------------------------------------------
    def onTrade(self, trade):
        if self.pos != 0:
            # 多头开仓成交后，撤销空头委托
            if self.pos > 0:
                for shortOrderID in self.shortOrderIDList:
                    self.cancelOrder(shortOrderID)
            # 反之同样
        elif self.pos < 0:
            for buyOrderID in self.buyOrderIDList:
                self.cancelOrder(buyOrderID)

        # 移除委托号
        for orderID in (self.buyOrderIDList + self.shortOrderIDList):
            if orderID in self.orderList:
                self.orderList.remove(orderID)

        # 发出状态更新事件
        self.putEvent()
```