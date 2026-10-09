```
Out[9]: pandas.core.index.Index
```

上证指数的交易信息数据（SHIndex）在Python中读取后是DataFrame类型，上证综指的收盘指数数据（ClcIndex）则是Series类型。这些数据形式不能直观体现时间序列的基本性质。为了更好地分析上证指收篮指数的一些特性，我们需要将其转化成时间序列类型。在Python中，时间序列数据同样是pandas包中的DataFrame或是Series，但其索引为日期类型。

```
#将收盘指数转换成时间序列格式
In [10]: ClcIndex.index=pd.to_datetime(ClIndex.index)

In [11]: ClIndex.head()
Out[11]:
Tzdt
2014-01-02 2109.387
2014-01-03 2083.136
2014-01-06 2045.709
2014-01-07 2047.317
2014-01-08 2044.340
Name: ClIndex, dtype: float64

#查看ClIndex的类型
In [12]: type(ClIndex)
Out[12]: pandas.core.series.Series

#ClIndex的index是日期数据
In [13]: type(ClIndex.index)
Out[13]: pandas.tseries.index.DateTimeIndex

#最后，绘制时间序列图
In [14]: ClIndex.plot()
Out[14]: <matplotlib.axes._subplots.AxesSubplot at 0x7f76be698d48>
```

# ## 22.3 选取特定日期的时间序列数据

对于一些时间序列数据，我们有时候只需要研究其中某一时间段的数据，并不一定要分析处理所有时间段的数据。如果原始数据为长达20年的日度数据，而我们只需要研究2015年1月1日以后的数据，那么我们就可以根据特定条件从时间序列数据中提取出子集。pandas包为我们筛选数据集提供了很多方便，接下来将举例说明如何根据需求快速有效地筛选出所需数据。

(1) 筛选出某一时间段内的数据，比如如果我们想要筛选出上证综指从2014年10月8日到2014年11月1日区间内的数据，可以在Python中输入如下的代码：

```
#获取2014年10月8日到11月1日的数据
In [15]: SHIndex.index=pd.to_datetime(SHIndex.index)

In [16]: SHIndexPart=SHIndex['2014-10-08':'2014-10-31']
#查看前两周数据
In [17]: SHIndexPart.head(n=2)
```