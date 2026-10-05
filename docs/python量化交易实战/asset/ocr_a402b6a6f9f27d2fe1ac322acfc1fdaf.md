```
data = history_n(symbol=symbol,frequency="1d",end_time=now,count=5
,fields="close,volume",df=True)
    vloume = data["volume"].values
    close = data["close"].values
    corr = np.corrcoef(vloume,close)
    return -corr
```

其中的 data 是获取数据后生成的一个数据表，volume 和 close 是其中对应的数据。在获取数据后，调用 NumPy 计算两个数据列对应的相关系数并返回相关系数的倒数。

# 2. 因子 53 : ((close - low) - (high - close)) / ( (close - low)的 9 日之前的值-当前值 )

其代码如下：

```
def alpha53(symbol,now):
    last_day = get_previous_trading_date("SHSE",now)
    data = history_n(symbol=symbol, frequency="1d", end_time=last_day, count=9,
fields="high,low,close", df=True)
    high = data["high"].values
    low = data["low"].values
    close = data["close"].values
    result = (2 * close[-1] - high[-1] - low[-1])/(high[-1] - low[-1])
    return result
```

这里需要注意的是，对于需要计算的序列，其中的日期是至关重要的，此因子的计算是采用昨天的收盘价与 9 日前的价差进行比较，因此使用的日期为上一个交易日为宜。

顺便说一下，在这里可能是读者第一次接触到未来函数这个概念，因为在回测时一般是默认计算上一个交易日的买卖点，因此建议读者在使用时使用 last_day。这样能够较好地防止有未来函数参与回测中。

# 3. 因子 78 : 对股票过去 5 天的成交量和最高价进行排名，再取相关系数

其代码如下：

```
from scipy.stats import rankdata

def alpha53(symbol,now):
    last_day = get_previous_trading_date("SHSE", now)
    data = history_n(symbol=symbol, frequency="1d", end_time=last_day, count=5,
fields="volume,high", df=True)
    high = data["high"].values
    volume = data["low"].values
    high_rank = rankdata(high)
    volume_rank = rankdata(volume)
    corr = np.corrcoef(high_rank,volume_rank)
    return corr
```