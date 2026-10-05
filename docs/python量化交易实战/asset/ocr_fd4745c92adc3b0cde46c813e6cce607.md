get_previous_trading_date 函数获取相同市场上一个交易日的日期。
	history_n 函数获取截至当前日期上一个交易日的所有周期数据，并以 pandas 格式返回。

```
open = np.asarray((data["open"].values))
high = np.asarray((data["high"].values))
low = np.asarray((data["low"].values))
close = np.asarray((data["close"].values))
```

获取的数据需要通过 NumPy 库包进行数据处理，这里通过调用关键字的方法获取各个值的序列，为下一步的计算做准备。

MACD 值的计算就显得较为简单，直接调用 Talib 库包中的函数值计算即可。

```
macd,_,_ = talib.MACD(close)
macd = macd[-1]
```

> 注意
这里的 MACD 返回的是 3 个数值序列，根据需要只要第一个即可。而 macd[-1]指的是获取当前序列最后一个数值，即根据上一个交易日获取的 MACD 值进行计算。

判断 MACD 值的情况，调用掘金量化库包进行买卖，其方法如下：

```
if macd > 0:
order_volume(symbol=context.symbol, volume=context.volume,
side=PositionSide_Long,
order_type=OrderType_Market, position_effect=PositionEffect_Open)
print("买入")
elif macd < 0:
order_volume(symbol=context.symbol, volume=context.volume,
side=PositionSide_Short,
order_type=OrderType_Market, position_effect=PositionEffect_Close)
print("卖出")
```

# 3. 主方法的设定

主方法中主要对账户信息、Python 代码的名称、执行的类型、起止时间、复权方法进行设定。代码段如下：

```
if __name__ == "__main__":
    run(
        strategy_id='********************',
        filename='MACD 不同时间.py',
        mode=MODE_BACKTEST,
        token='9e02621e7fcf850731def2e8a9de1fdbd5b21ad6',
        backtest_start_time='2017-01-01 09:00:00',
        backtest_end_time='2017-12-31 15:00:00',
        backtest_initial_cash=20000,
```