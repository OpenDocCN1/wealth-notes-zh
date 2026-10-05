```
```
am = self.am

# 更新K线到时间序列容器中
am.update_bar(bar)

# 若缓存的K线数量尚不够计算技术指标，则直接返回
if not am.inited:
    return

# 计算快速均线
fast_ma = am.sma(self.fast_window, array=True)
self.fast_ma0 = fast_ma[-1]      # T时刻数值
self.fast_ma1 = fast_ma[-2]      # T-1时刻数值

# 计算慢速均线
slow_ma = am.sma(self.slow_window, array=True)
self.slow_ma0 = slow_ma[-1]
self.slow_ma1 = slow_ma[-2]

# 判断是否金叉
cross_over = (self.fast_ma0 > self.slow_ma0 and
             self.fast_ma1 < self.slow_ma1)

# 判断是否死叉
cross_below = (self.fast_ma0 < self.slow_ma0 and
              self.fast_ma1 > self.slow_ma1)

# 如果发生了金叉
if cross_over:
    # 那么为了保证成交，在K线收盘价上加5发出限价单
    price = bar.close_price + 5

    # 若当前无仓位，则直接开多
    if self.pos == 0:
        self.buy(price, 1)
    # 若当前持有空头仓位，则先平空，再开多
    elif self.pos < 0:
        self.cover(price, 1)
        self.buy(price, 1)

# 如果发生了死叉
elif cross_below:
    price = bar.close_price - 5

    # 若当前无仓位，则直接开空
    if self.pos == 0:
        self.short(price, 1)
```
```