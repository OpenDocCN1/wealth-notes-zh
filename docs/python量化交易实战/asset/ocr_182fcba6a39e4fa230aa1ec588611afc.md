```
last_day = get_previous_trading_date("SHSE",now)
day_time ,hour_and_mins = str(now).split(" ")

symbol_list = []
if hour_and_mins == "09:31:00":
    _symbol_list = tools.get_symbol_list(context.index,now)

    for symbol in _symbol_list:
        data =
history_n(symbol,frequency="1d",fields="close",count=14,end_time=last_day)
        close = tools.get_data_value(data,"close")
        std = np.std(close)
        if std < context.threshold:
            symbol_list.append(symbol)
    print("----------",now," ",symbol_list,"----------")

for symbol in symbol_list:
    volume = history_n(symbol=symbol, frequency="60s", count=2,
fields='volume', fill_missing='Last',
            end_time=now, df=True)['volume'].values
    if volume[1]/volume[0] >= 5:
        current_price = current(symbol)[0]["price"]
        last_price = history_n(symbol=symbol, frequency="1d", count=1,
fields='close', fill_missing='Last',
            end_time=now, df=True)['close'].values
        if np.log(current_price/last_price) > 0:
            order_target_percent(symbol=symbol, percent=1. / (context.num),
order_type=OrderType_Market,
                position_side=PositionSide_Long)
            context.symbol_buy_pirce[symbol] = current_price

if __name__ == "__main__":
    run(
        strategy_id='9f53e15f-6870-11e8-801c-4cedfb681747',
        filename=(os.path.basename(__file__)),
        mode=MODE_BACKTEST,
        token='715f84a04a4574e2e77491d62f7c8bbc0c663b12',
        backtest_start_time="2018-01-01 09:30:00",
        backtest_end_time='2018-01-15 15:00:00',
        backtest_initial_cash=10000000,
        backtest_adjust=ADJUST_PREV
    )
```