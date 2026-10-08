```
# 简单的描述性统计分析
```

|  | openPrice | highestPrice | lowestPrice | closePrice | turnoverVol | Adj_closePrice |
|---|---|---|---|---|---|---|
| count | 3492.000000 | 3492.000000 | 3492.000000 | 3492.000000 | 3.492000e+03 | 3492.000000 |
| mean | 4.706982 | 4.795951 | 4.625495 | 4.771217 | 1.575593e+08 | 6.615731 |
| std | 1.987839 | 2.053475 | 1.926265 | 1.937816 | 1.721244e+08 | 3.076444 |
| min | 0.000000 | 0.000000 | 0.000000 | 2.200000 | 0.000000e+00 | 2.586000 |
| 25% | 3.230000 | 3.270000 | 3.200000 | 3.240000 | 5.666951e+07 | 3.861000 |
| 50% | 4.295000 | 4.380000 | 4.250000 | 4.350000 | 9.853506e+07 | 6.325500 |
| 75% | 5.750000 | 5.860000 | 5.650000 | 5.780000 | 1.852211e+08 | 8.320000 |
| max | 13.130000 | 13.500000 | 12.850000 | 13.080000 | 1.721668e+09 | 18.403000 |

```
# 基本信息
```python
data_new1.info()
```
# 查看全部
```
<class 'pandas.core.frame.DataFrame'>
Index: 3492 entries, 2002-09-10 to 2017-02-23
Data columns (total 9 columns):
secID_x				3492 non-null object
secShortName			3492 non-null object
openPrice				3492 non-null float64
highestPrice			3492 non-null float64
lowestPrice			3492 non-null float64
closePrice			3492 non-null float64
turnoverVol			3492 non-null int64
secID_y				3492 non-null object
Adj_closePrice		3492 non-null float64
dtypes: float64(5), int64(1), object(3)
memory usage: 272.8 + KB
```
```

```
# 股票收盘价走势(图 8-1)
```python
data_new1['Adj_closePrice'].plot(legend = True, figsize= (14, 6))
```
```

![](bbox=[0.17, 0.6, 0.84, 0.83])

图 8-1 股票收盘价走势

```
# 成交量走势(图 8-2)
```python
data_new1['turnoverVol'].plot(legend = True, figsize= (14, 6))
```
```