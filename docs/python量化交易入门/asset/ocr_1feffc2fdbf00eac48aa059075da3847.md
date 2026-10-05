+   - cancelAll(self, name)：全部撤单，包括未成交的限价单和停止单。
- saveSyncData(self, strategy)：保存同步数据。
- getPriceTick(self, strategy)：获取最小价格变动。

# 4. 结果计算

+   - calculateBacktestingResult(self)：逐条匹配交易，计算每笔交易的盈亏。基于每笔交易的结果，计算具体的盈亏曲线和最大回撤等，然后再计算盈亏相关数据，最后返回回测结果。
- showBacktestingResult(self)：显示回测结果，输出回测报告，并显示回测结果图，包括资金子图、回撤子图、每笔交易盈亏子图和每笔盈亏分布子图。
- clearBacktestingResult(self)：清空之前的回测结果，包括相关的限价单、相关的停止单和相关成交。
- runOptimization(self, strategyClass, optimizationSetting)：运行普通模式的优化参数，也就是说 Python 主进程循环回测每一个参数组合，并输出每个参数组合的优化结果。普通模式下只能使用 CPU 的一个核。
- runParallelOptimization(self, strategyClass, optimizationSetting)：运行多进程模式的优化参数，也就是说并行运行多个进程回测每一个参数组合，并输出每个参数组合的优化结果。
- updateDailyClose(self, dt, price)：更新每日收盘价。
- calculateDailyResult(self)：按照逐日盯市的方式计算每日的交易盈亏和持仓盈亏，并汇总成最终按日统计的盈亏情况。
- calculateDailyStatistics(self, df)：按照逐日盯市的方式统计每日的交易盈亏和持仓盈亏，并且计算盈亏相关数据，如总收益、年化收益、最大回撤、日均盈亏和夏普比率等。
- showDailyResult(self, df=None, result=None)：显示按照逐日盯市方式统计出的回测结果，并显示回测结果图，包括资金子图、回撤子图、每日盈亏子图和每日盈亏分布子图。