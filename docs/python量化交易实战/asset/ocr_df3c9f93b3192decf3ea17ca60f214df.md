价差偏离度和传统的市值因子、估值因子相关性弱，通过因子分层后分组和 Fama-Macbeth 回归可以发现，传统的 1 个月反转和 3 个月反转可以被价差偏离度替代，而价差偏离度因子信息来源相对独立，不能被其他的常见因子所解释。

因此可以知道，价差偏离度因子的主要作用是捕捉股票相对其同类型（板块）股票的高估低估程度。低估的股票之后会补涨，高估的股票之后有补跌的趋势。

下面介绍价差偏离度因子的使用。

（1）价差偏离度因子指的是股票池内股票标的过去 250 个交易日的复权收盘价，要求计算涨跌幅及 250 天涨跌幅的 Pearson 相关系数，而“股票间的距离”使用的是皮尔逊相关系数，用系数値代替不同的股票之间的距离。

公式如下：

Distance=两股票过去 250 个交易日涨跌幅的 Pearson 相关系数 distance
=1-两股票过去 250 个交易日涨跌幅的 Pearson 相关系数

（2）为每一只股票取出距离最近的 10 只股票 distance_min10，等权构建这 10 只股票的特征组合，计算特征组合的净值价格，作为参考价格（ReferencePrice）。

（3）取出股票池中每只股票的收盘价（closePrice），与参考价格取对数后作差，得到对数价差（PriceSpread）。

公式如下：

PriceSpread=ln((closePrice)-ln(ReferencePrice))PriceSpread
=ln((closePrice)-ln(ReferencePrice))

（4）对每只股票算过去 60 个交易日的 PriceSpread，并对其标准化得到 SpreadBias。公式如下：

SpreadBias=PriceSpread-mean(PriceSpread)std(PriceSpread)SpreadBias
=PriceSpread-mean(PriceSpread)std(PriceSpread)

股票池中距离的获取：

```
def get_max_symbols_list(symbol_list = "",frequency="1d",count = 250,now = "",num = 10):

    start_date = ago.get_previous_date(now,250)
    last_day = get_previous_trading_date("SZSE",now)

    df = pd.DataFrame([])
    _symbol_list = []
    for symbol in symbol_list:
        try:
            data = history(symbol=symbol, frequency=frequency,
```