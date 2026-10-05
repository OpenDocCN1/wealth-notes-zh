# 16.6.1  on_tick的逻辑

on_tick属于回调函数。所谓回调函数，大致可以理解为不是用户主动调用的函数，而是由对应的服务端调用的函数。在这里，on_tick函数就是每当产生一个新的行情数据的时候，就会调用该函数，函数的参数就是对应的tick信息。

strategyDualThrust.py主要是完成了计算K线的工作。然后，在产生一个新的K线的时候，调用on_bar，具体的策略逻辑是写在on_bar里面的。也就是说，策略的计算信号的频率是以K线为单位的。

主要代码如下：

```
if tickMinute != self.barMinute:
    if self.bar:
        self.on_bar(self.bar)

    bar = VtBarData()
    bar.vtSymbol = tick.vtSymbol
    bar.symbol = tick.symbol
    bar.exchange = tick.exchange

    bar.open = tick.lastPrice
    bar.high = tick.lastPrice
    bar.low = tick.lastPrice
    bar.close = tick.lastPrice

    bar.date = tick.date
    bar.time = tick.time
    bar.datetime = tick.datetime    # K线的时间设为第一个Tick的时间
```