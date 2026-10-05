+   - onStart：策略启动方法，一般做一些启动提示。
- onStop：策略停止方法，如撤单、平今仓转平昨仓、结算收盘等。
- onTick(self, tick)：当最新 tick 数据更新时，做一些计算，信号触发，当然也可以做撤单。收盘后的一段时间可能收到垃圾数据（因为有时柜台系统会做一些调试），建议退出账户，也可在策略或者引擎里自行处理。以 tick 为实例，获取属性请使用对象方法，如 “tick.lastPrice”，下面的 order、trade、bar 皆如此。
- onOrder(self, order)：当发出委托后，就会收到关于委托状态的信息，无论是否成交，可以根据回报的信息来进行撤单、追单等处理。
- onTrade(self, trade)：成交数据回报。主要用来计算持仓，也可以用来触发委托、追单等操作。
- onBar(self, bar)：该方法一般在 onTick 里被调用，当生成新 K 线的时候，触发该方法，进而触发基于 K 线的策略信号。

# 4.4.4 主动函数

主动函数如下所述。

+   - buy(self, price, volume, stop=False)：开仓做多，默认合约为 self.vtSymbol，stop 为 False 直接发市价单，为 True 时发本地停止单，默认 False。
- sell(self, price, volume, stop=False)：平多仓，默认市价单平仓。
- short(self, price, volume, stop=False)：开仓做空，默认市价单开仓。
- cover(self, price, volume, stop=False)：平空仓，默认市价单平仓。
- sendOrder(self, orderType, price, volume, stop=False)：区别对待本地停止单和直接发市价单。所以该函数是前面 buy，sell，short，cover 函数的基础，二次封装后被其调用。
- cancelOrder(self, vtOrderID)：根据发单编号进行撤单，该方法根据 vtOrderID 来判断撤本地单子，还是撤交易所排队的单子。
- insertTick(self, tick)：向数据库中插入 tick 数据。对 ctaEngine 函数的二次封装，