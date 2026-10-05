```
super(CciSignal, self).__init__()

self.bg = BarGenerator(self.onBar)
self.am = ArrayManager()

self.cciWindow = cciWindow
self.cciLevel =cciLevel
self.cciLong = 50+self.cciLevel
self.cciShort = 50-self.cciLevel

#----------------------------------------------------------------
def onTick(self, tick):
    """Tick 更新"""
    self.bg.updateTick(tick)

#----------------------------------------------------------------
def onBar(self, bar):
    """K线更新"""
    self.am.updateBar(bar)

    if not self.am.inited:
        self.setSignalPos(0)

    cciValue = self.am.cci(self.cciWindow)

    if cciValue >= self.cciLong:
        self.setSignalPos(1)
    elif cciValue<= self.cciShort:
        self.setSignalPos(-1)
    else:
        self.setSignalPos(0)
```

在交易管理部分，如 MultiSignalStrategy 类，则需要增加信号部分的策略参数，并且把该参数名称添加到参数列表 paraList 中，以便优化。

然后在 onBar 函数中传入定义好的策略参数即可，其示例代码如下：

```
......
# 策略参数
rsiWindow= 14
rsiLevel = 20
```