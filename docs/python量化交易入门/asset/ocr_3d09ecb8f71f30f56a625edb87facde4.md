缓存起来，故其投资组合就是 3 个 DataFrame 相加即可，即 dfp = df1 + df2 + df3，然后去掉空值 dropa()。

然后创建回测引擎，设置回测投资组合策略要用到的起始资金，用 calculateDailyStatistics()计算逐日统计的结果，然后通过 showDailyResult()显示回测结果。

```
# 合并获得组合回测结果
dfp = df1+ df2 + df3

# 注意如果被抛弃的交易日位于回测的前后，即两者不重合的日期中，则不会影响组合曲线的正确性
# 但是如果被抛弃的交易日位于回测的中部，即两者重合的日期中，则组合曲线会出现错误（丢失交易日）
dfp = dfp.dropna()

# 创建回测引擎，并设置组合回测初始资金后，显示结果
engine = BacktestingEngine()
engine.setCapital(1000000)     #本金为100万元
dfp, result = engine.calculateDailyStatistics(dfp)
engine.showDailyResult(dfp, result)
```

逐日统计结果显示多策略组合，其年化收益为 22.46%，百分比最大回撤 -2.48%，夏普比率达 2.53，如图 7-7 所示。由于交易品种相关性不高，有利于抵消了各自的非系统性风险，投资组合的夏普比率高于单个品种的夏普比率。

![](bbox=[0.17, 0.63, 0.77, 0.87])