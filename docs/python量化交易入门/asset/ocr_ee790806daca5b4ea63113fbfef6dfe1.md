```
self.trade()

#-------------------------------------------------------------------------------
def trade(self):
    """执行交易"""
    # 先撤销之前的委托
    self.cancelAll()

    # 如果目标仓位和实际仓位一致，则不进行任何操作
    posChange = self.targetPos - self.pos
    if not posChange:
        return

    # 确定委托基准价格，有Tick数据时优先使用，否则使用Bar数据
    longPrice = 0
    shortPrice = 0

    if self.lastTick:
        if posChange > 0:
            longPrice = self.lastTick.askPrice1 + self.tickAdd
            if self.lastTick.upperLimit:
                longPrice = min(longPrice, self.lastTick.upperLimit)
# 涨停价检查
        else:
            shortPrice = self.lastTick.bidPrice1 - self.tickAdd
            if self.lastTick.lowerLimit:
                shortPrice = max(shortPrice, self.lastTick.lowerLimit)
# 跌停价检查
    else:
        if posChange > 0:
            longPrice = self.lastBar.close + self.tickAdd
        else:
            shortPrice = self.lastBar.close - self.tickAdd

    # 回测模式下，采用合并平仓和反向开仓委托的方式
    if self.getEngineType() == ENGINETYPE_BACKTESTING:
        if posChange > 0:
            l = self.buy(longPrice, abs(posChange))
        else:
            l = self.short(shortPrice, abs(posChange))
        self.orderList.extend(l)
```