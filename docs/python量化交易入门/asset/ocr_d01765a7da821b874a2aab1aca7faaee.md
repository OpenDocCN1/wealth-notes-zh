最高价 max(self.bar.high, tick.lastPrice)表示缓存进来的 bar.high 与每一次新进来的 Tick 对比，取最大值。同理，最低价 min(self.bar.low, tick.lastPrice)表示缓存进来的 bar.low 与每一次新进来的 Tick 对比，取最小值。

```
# 初始化新1分钟的K线数据
    if newMinute:
        self.bar.vtSymbol = tick.vtSymbol
        self.bar.symbol = tick.symbol
        self.bar.exchange = tick.exchange
        self.bar.open = tick.lastPrice
        self.bar.high = tick.lastPrice
        self.bar.low = tick.lastPrice
    # 累加更新老1分钟的K线数据
    else:
        self.bar.high = max(self.bar.high, tick.lastPrice)
        self.bar.low = min(self.bar.low, tick.lastPrice)
```

最后步骤是通用部分的更新，分别缓存收盘价、日期时间(如 20180717 10:01:00.000000 )和未平仓合约。

self.bar.volume += max(volumeChange, 0)的作用是每一次 Tick 推送的时候把成交的变化量累加过来，max(volumeChange, 0)的作用是把负的成交量过滤掉。出现负的成交量的原因是夜盘开盘 lastTick.volume 为昨日收盘数据。

```
# 通用更新部分
    self.bar.close = tick.lastPrice
    self.bar.datetime = tick.datetime
    self.bar.openInterest = tick.openInterest
    if self.lastTick:
        volumeChange = tick.volume - self.lastTick.volume  # 当前K线内的成交量
        self.bar.volume += max(volumeChange, 0)
# 避免夜盘开盘lastTick.volume为昨日收盘数据，导致成交量变化为负的情况
    # 缓存Tick
    self.lastTick = tick
```