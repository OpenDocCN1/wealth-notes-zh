| 简称 | 函数名 |
|---|---|
| SAR | Parabolic SAR |
| SAREXT | Parabolic SAR - Extended |
| SMA | Simple Moving Average |
| STDDEV | Standard Deviation |
| STOCH | Stochastic |
| STOCHF | Stochastic Fast |
| STOCHRSI | Stochastic Relative Strength Index |
| SUM | Summation |
| T3 | Triple Exponential Moving Average (T3) |
| TEMA | Triple Exponential Moving Average |
| TRANGE | True Range |
| TRIMA | Triangular Moving Average |
| TRIX | 1-day Rate-Of-Change (ROC) of a Triple Smooth EMA |
| TSF | Time Series Forecast |
| TYPPRICE | Typical Price |
| ULTOSC | Ultimate Oscillator |
| VAR | Variance |
| WCLPRICE | Weighted Close Price |
| WILLR | Williams' %R |
| WMA | Weighted Moving Average |

表 5-1 中提供了 Talib 相关函数，接下来会对部分内容进行介绍。

# 5.2.1 Talib 常用函数介绍

# 1.平均趋向指数 ADX 和累积派发线 AD

AD（累积派发线）是一种平衡交易量指标，以当日的收盘价位来估算成交流量，用于估算一段时间内该证券累积的资金流量。它通常与 ADX 一起使用，利用多空趋向的变化差离与总和判定平均趋势，ADX 数值不能显示趋势的发展方向。但是如果趋势存在，ADX 可以衡量趋势的强度。

ADX 与 AD 的函数如下：

```
def ADX(high, low, close, *args, **kwargs):
```