# **第 7 章**

# **金融时间序列分析及其 Python 应用**

# **7.1 时间序列分析的基础知识**

# **7.1.1 时间序列的概念及其特征**

对一个或者一组变量 $x(t)$ 进行观察测量，将在一系列时刻 $t_1, t_2, \cdots, t_n$ 所得到的离散数字组成的序列集合称为时间序列。例如，某股票从 2015 年 6 月 1 日到 2016 年 6 月 1 日之间各个交易日的收盘价可以构成一个时间序列，某地每天的最高气温可以构成一个时间序列。

时间序列具有以下特征：

+   * （1）趋势：在长时期内呈现出持续向上或持续向下的变动。
* （2）季节变动：在一年内重复出现的周期性波动。如气候条件、生产条件、节假日或人们的风俗习惯等各种因素导致的结果。
* （3）循环波动：呈现非固定长度的周期性波动。循环波动的周期可能会持续一段时间，但与趋势不同，它不是朝着单一方向的持续变动，而是涨落相同的交替波动。
* （4）不规则波动：除去趋势、季节变动和周期波动之后的随机波动的时间序列。不规则波动通常总是混杂在时间序列中，致使时间序列产生一种波形或震荡式的变动。只含有随机波动的序列也称为平稳序列。

# **7.1.2 平稳性**

如果一个时间序列的均值没有系统性的变化（无趋势）、方差没有系统性的变化，且严格消除了周期性变化，就说它是平稳的。

先通过如下代码生成图 7-1。

```
```python
IndexData = DataAPI.MktIdxGet(indexID = u"", ticker = u"000001", beginDate = u"20130101", endDate = u"20140801", field = u"tradeDate,closeIndex,CHGPct", pandas = "1")
IndexData = IndexData.set_index(IndexData['tradeDate'])
IndexData['closeIndexDiff_1'] = IndexData['closeIndex'].diff(1)  # 1 阶差分处理
IndexData['closeIndexDiff_2'] = IndexData['closeIndexDiff_1'].diff(1)  # 2 阶差分处理
IndexData.plot(subplots = True, figsize = (18,12))
```
```