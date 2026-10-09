```
class BacktestingBaseRM(BacktestingBase):
    def set_prices(self, price):
        ''' 设置用来进行性能回溯的价格，比如测试跟踪止损是否命中。'''
        self.entry_price = price ①
        self.min_price = price ②
        self.max_price = price ③

    def place_buy_order(self, bar, amount=None, units=None, gprice=None):
        ''' 对于给定的bar和给定的amount或给定的units数量下一个买入单。'''
        date, price = self.get_date_price(bar)
        if gprice is not None:
            price = gprice
        if units is None:
            units = int(amount / price)
        self.current_balance -= (1 + self.ptc) * units * price + self.ftc
        self.units += units
        self.trades += 1
        self.set_prices(price) ④
        if self.verbosel:
            print(f'{date} | buy {units} units for {price:.4f}')
            self.print_balance(bar)

    def place_sell_order(self, bar, amount=None, units=None, gprice=None):
        ''' 对于给定的bar和给定的amount或给定的units数量下一个卖出单。'''
        date, price = self.get_date_price(bar)
        if gprice is not None:
            price = gprice
        if units is None:
            units = int(amount / price)
        self.current_balance += (1 - self.ptc) * units * price - self.ftc
        self.units -= units
        self.trades += 1
        self.set_prices(price) ④
        if self.verbosel:
            print(f'{date} | sell {units} units for {price:.4f}')
            self.print_balance(bar)
```

+   ① 设置最近交易的进入价格。
② 设置自最近交易以来的初始最低价格。
③ 设置自最近交易以来的初始最高价格。
④ 设置交易执行后的相关价格。

基于这个新的基类，11.7.4 节提供了一个新的回测类 TBBacktesterRM，它允许包含止损单、跟踪止损单和止盈单。相关代码部分将在接下来的内容中进行讨论。如上节所计算的，回溯示例的参数将定位在大约2%的ATR水平上。