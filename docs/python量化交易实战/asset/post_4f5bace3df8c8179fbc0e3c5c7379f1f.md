
return (trading_periods * v.mean()) ** 0.5

result = rs.rolling(window=window, center=False).apply(func=f)
if clean:
    return result.dropna()
else:
    return result

（3）Parkinson 和 Garman-Klass 估计量之所以能提高估计效率，是因为它们依赖于一些不适用于真实市场的假设，尤其是价格服从不带漂移项的几何布朗运动以及交易是连续的假设。Rogers-Satchell 在一定程度上放宽了这些限制条件，引入了带有漂移项的更优的估计量。

```
def RogersSatchell_get_estimator(price_data, window=30, trading_periods=252, clean=True):

    log_ho = (price_data['High'] / price_data['Open']).apply(np.log)
    log_lo = (price_data['Low'] / price_data['Open']).apply(np.log)
    log_co = (price_data['Close'] / price_data['Open']).apply(np.log)

    rs = log_ho * (log_ho - log_co) + log_lo * (log_lo - log_co)

    def f(v):
        return trading_periods * v.mean() ** 0.5

    result = rs.rolling(
        window=window,
        center=False
    ).apply(func=f)

    if clean:
        return result.dropna()
    else:
        return result
```

（4）Garman-Klass（1980）估计量无法解决价格序列中存在跳空开盘的情况。Yang-Zhang（2000）推导出了适用于价格跳空开盘的估计量，本质上是各种估计量的加权平均。

```
def YangZhang_get_estimator(price_data, window=30, trading_periods=252, clean=True):

    log_ho = (price_data['High'] / price_data['Open']).apply(np.log)
    log_lo = (price_data['Low'] / price_data['Open']).apply(np.log)
    log_co = (price_data['Close'] / price_data['Open']).apply(np.log)

    log_oc = (price_data['Open'] / price_data['Close'].shift(1)).apply(np.log)

log_oc_sq = log_oc ** 2

log_cc = (price_data['Close'] / price_data['Close'].shift(1)).apply(np.log)
log_cc_sq = log_cc ** 2

rs = log_ho * (log_ho - log_co) + log_lo * (log_lo - log_co)

close_vol = log_cc_sq.rolling(
    window=window,
    center=False
).sum() * (1.0 / (window - 1.0))
open_vol = log_oc_sq.rolling(
    window=window,
    center=False
).sum() * (1.0 / (window - 1.0))
window_rs = rs.rolling(
    window=window,
    center=False
).sum() * (1.0 / (window - 1.0))

k = 0.34 / (1 + (window + 1) / (window - 1))
result = (open_vol + k * close_vol + (1 - k) * window_rs).apply(np.sqrt) * math.sqrt(trading_periods)

if clean:
    return result.dropna()
else:
    return result
```

（5）使用偏度 Skew 代替波动率。

```
def Skew_get_estimator(price_data, window=30, clean=True):
    log_return = (price_data['Close'] /
price_data['Close'].shift(1)).apply(np.log)

    result = log_return.rolling(
        window=window,
        center=False
    ).skew()

    if clean:
        return result.dropna()
    else:
        return result
```

（6）使用峰度 Kurtosis 代替波动率。

```
def Kurtosis_get_estimator(price_data, window=30, clean=True):
    "峰度 Kurtosis 代替波动率"
    log_return = (price_data['Close'] /
    price_data['Close'].shift(1)).apply(np.log)

    result = log_return.rolling(
        window=window,
        center=False
    ).kurt()

    if clean:
        return result.dropna()
    else:
        return result
```

## 7.4 实战：一个回测成功率 100% 的中长线买卖例子

对于股票投资，特别是中长线投资，成功率是非常重要的，任何人都不希望经过长时间的投资等待，原先的一笔投资产生亏损。下面给读者分享一个回测中成功率能够达到 100% 的选股策略，主要依靠的是技术选股，如图 7-7 所示。

![](img/66b32d04cb77b904cead314891984c93_168_0.png)

图 7-7 回测成功率 100%的选股策略

### 7.4.1 技术指标的设计

传统的选股方式主要依靠技术指标，这也是长期以来广大股民从市场中获取收益的方式。技术指标更多的是涉及对收盘价以及交易量的波动性判断。

首先是买入点的指标，代码如下：

（1）定义 60 天的波动率小于 10%：

```
def buy_check_price_amplitude(context, symbol, count = 60, threshold = 0.1):
    now = context.now
    last_day = get_previous_trading_date("SHSE", now)
    #amplitude = beta_and_vol.get_volatility_normal(symbol,last_day,count=count)
    data = history_n(symbol,frequency="1d",count=count,end_time=last_day,fields="open,high,low,close")
    close = msci_tools.get_data_value(data,"close")
    open = msci_tools.get_data_value(data, "open")
    high = msci_tools.get_data_value(data, "high")
    low = msci_tools.get_data_value(data, "low")
    amplitude = (np.max(high) - np.min(low))/close[0]
    if amplitude < threshold:
        return True
    else:
        return False
```

（2）买入点-地量（连续 18 周成交量小于最近 4 年内天量的 10%）：

```
def buy_check_mean_volume_small(context,
    symbol,week_count_short=18 ,week_count_long = 150, threshold = 0.1):
    now = context.now
    last_day = get_previous_trading_date("SHSE", now)
    data = history_n(symbol, frequency="1d", count=week_count_long * 5, end_time=last_day, fields="volume")
    volume = msci_tools.get_data_value(data,"volume")
    max_volume_last = np.max(volume[-week_count_short*5:])
    max_volume = np.max(volume)
    if max_volume_last < max_volume * 0.1:
        return True
    else:
        return False
```

（3）单日成交量大于该股的前五日移动平均成交量的 2.5 倍，大于前 10 日移动平均成交量的 3 倍：

```
def buy_check_volume_increase(context, symbol):
    now = context.now
    last_day = get_previous_trading_date("SHSE", now)
    data = history_n(symbol, frequency="1d", count=11, end_time=last_day, fields="volume")
    volume = msci_tools.get_data_value(data,"volume")
    h = volume
    volume = h['volume'][-1]
    volume_mean_5 = np.mean(h['volume'][-6:-1])
    volume_mean_10 = np.mean(h['volume'][-11:-1])
    if volume > volume_mean_5 * 2.5 and volume > volume_mean_10 * 3:
        return True
    else:
        return False
```

（4）找到昨天之前成交量大于昨天的成交量（0.8 倍），这个区间的天数大于 30 天，昨天单日成交量大于该区间的平均成交量的 2 倍，区间价格波动小于 10%：

```
def buy_check_volume_increase_and_price_amplitude(context, symbol):
    now = context.now
    last_day = get_previous_trading_date("SHSE", now)
    data = history_n(symbol, frequency="1d", count=270, end_time=last_day, fields="open,high,low,close,volume")
    close = msci_tools.get_data_value(data,"close")
    open = msci_tools.get_data_value(data, "open")
    high = msci_tools.get_data_value(data, "high")
    low = msci_tools.get_data_value(data, "low")
    volume = msci_tools.get_data_value(data,"volume")
    volume_yesterday = volume[-1]
    # 昨日收盘价要高于开盘价
    if close[-1] < open[-1]:
        return False
    #找到昨天之前成交量大于昨天的成交量（0.8倍）的那个日期，这个日期到今天的区间的天数大于30天
    start = 0
    end = -1
    for i in range(1, len(volume)):
        index = -(i + 1)
        if volume[index] > volume[-1] * 0.8:
            start = index + 1
            break
    if start == 0 or (end - start) < 30:
        return False
    # 昨天单日成交量大于该区间的平均成交量的2倍
    volume_mean = np.mean(volume[start:end])
    volume_mean_5 = np.mean(volume[-6:-1])
    if volume_yesterday < volume_mean * 2.5 or volume_yesterday < volume_mean_5 * 2.5:
        return False
    # 区间价格波动小于10%
    price_max = max(high[start:end])
    price_min = min(low[start:end])
    price_amplitude = (price_max - price_min) / open[start]
    # print 'min=%s, max=%s, open=%s, amplitude=%s' % (price_min, price_max, h['open'][start], price_amplitude)
    if price_amplitude > 0.1:
        return False
    return True
```

（5）对于卖出点的指标设定，卖出点的判定代码如下：

```
def sell_check(context, symbol):
    if sell_check_mean_price(context, symbol, 0.1) and sell_check_turnover_ratio(context, symbol):
        return True
    if sell_check_mean_price(context, symbol, 0.2):
        return True
    return False
```

（6）卖出点-均线（5日线超过10日线10%，10日线超过30日线10%）：

```
def sell_check_mean_price(context, symbol, threshold = 0.1):
    now = context.now
    last_day = get_previous_trading_date("SHSE", now)

    data = history_n(symbol, frequency="1d", count=31, end_time=last_day,
fields="open,high,low,close,volume")

    close = msci_tools.get_data_value(data, "close")
    open = msci_tools.get_data_value(data, "open")
    high = msci_tools.get_data_value(data, "high")
    low = msci_tools.get_data_value(data, "low")
    volume = msci_tools.get_data_value(data, "volume")

    # 求 5 日线、10 日线、30 日线
    mean_5 = np.mean(close[-5:])
    mean_10 = np.mean(close[-10:])
    mean_30 = np.mean(close[-30:])
    diff_5_10 = (mean_5 - mean_10) / mean_10
    diff_5_30 = (mean_5 - mean_30) / mean_30
    diff_10_30 = (mean_10 - mean_30) / mean_30

    # 求昨天的 5 日线、10 日线、30 日线
    yes_mean_5 = np.mean(close[-6:-1])
    yes_mean_10 = np.mean(close[-11:-1])
    yes_mean_30 = np.mean(close[-31:-1])
    yes_diff_5_10 = (yes_mean_5 - yes_mean_10) / yes_mean_10
    yes_diff_5_30 = (yes_mean_5 - yes_mean_30) / yes_mean_30
    yes_diff_10_30 = (yes_mean_10 - yes_mean_30) / yes_mean_30

    return diff_5_30 > threshold
```

（7）卖出点-换手率：

```
def sell_check_turnover_ratio(context, symbol):

    now = context.now
    last_day = get_previous_trading_date("SHSE", now)
    #换手率
    _df = get_fundamentals(table='trading_derivative_indicator', symbols=symbol,
start_date=last_day,
            end_date=last_day, fields='TURNRATE')

    TURNRATE = msci_tools.get_data_value(_df,"TURNRATE")
    return TURNRATE[0]>15

#卖出点 RSRS
```
