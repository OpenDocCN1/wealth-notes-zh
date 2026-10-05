# 16.5.5  实盘K线合成

DemoStrategy的双均线交易逻辑，可以通过超价买卖的方式来保证成交，从而忽略撤单、委托更新、成交推送之类更加细节的事件驱动逻辑。

但在进行实盘交易时，任何交易系统（不管是期货CTP、还是数字货币BITMEX等）都只会推送最新的Tick更新数据，而不会有完整的K线推送，因此用户需要自行在本地完成Tick到K线的合成逻辑。

vn.py也提供了完善的K线合成工具BarGenerator，用户只需在策略的__init__函数中创建实例即可，代码如下：

```
# K线合成器：将Tick数据合成分钟K线
self.bg = BarGenerator(self.on_bar)
```

其中，BarGenerator对象创建时，传入的参数（self.on_bar）是指当1分钟K线走完时所触发的回调函数。

在实盘策略收到最新的Tick推送时，我们只需要将TickData更新到BarGenerator中即可，代码如下：

```
def on_tick(self, tick: TickData):
    """
    通过该函数接收Tick推送。
```