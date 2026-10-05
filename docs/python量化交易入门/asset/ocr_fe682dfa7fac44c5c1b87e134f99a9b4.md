calculateTargetPos 函数：把各因子产生的信号仓位缓存到信号字典，遍历信号字典计算最终的目标持仓，最后调用 setTargetPos 函数，传到底层的目标持仓算法进行具体委托交易。

```
class MultiSignalStrategy(TargetPosTemplate):
.....
    #---------------------------------------------------------------------
    def __init__(self, ctaEngine, setting):
        """Constructor"""
        super(MultiSignalStrategy, self).__init__(ctaEngine, setting)

        self.rsiSignal = RsiSignal()
        self.cciSignal = CciSignal()
        self.maSignal = MaSignal()

        self.signalPos = {
            "rsi": 0,
            "cci": 0,
            "ma": 0
        }
    .....
    #---------------------------------------------------------------------
    def onTick(self, tick):
        """收到行情Tick推送(必须由用户继承实现)"""
        super(MultiSignalStrategy, self).onTick(tick)

        self.rsiSignal.onTick(tick)
        self.cciSignal.onTick(tick)
        self.maSignal.onTick(tick)

        self.calculateTargetPos()
    #---------------------------------------------------------------------
    def onBar(self, bar):
        """收到Bar推送(必须由用户继承实现)"""
        super(MultiSignalStrategy, self).onBar(bar)

        self.rsiSignal.onBar(bar)
        self.cciSignal.onBar(bar)
```