```
return (trading_periods * v.mean()) ** 0.5

result = rs.rolling(window=window, center=False).apply(func=f)
if clean:
    return result.dropna()
else:
    return result
```

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
```