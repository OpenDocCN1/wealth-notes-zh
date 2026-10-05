# 3.2.5 根据已有的列建立新列

本节将根据现有的代码列，加入该股票代码对应的行业列：

```
In [67]: data.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 6 entries, 0 to 5
Data columns (total 5 columns):
Unnamed: 0      6 non-null int64
ticker          6 non-null int64
secShortName    6 non-null object
tradeDate       6 non-null object
closePrice      6 non-null float64
dtypes: float64(1), int64(2), object(2)
memory usage: 312.0+ bytes
```

```
In [68]: a=pd.read_csv('closeprice.csv',encoding='gbk')
In [69]: a
Out[69]:
Unnamed: 0  ticker  secShortName  tradeDate  closePrice
0  0  1  平安银行  2017-06-20  9.12
1  1  2  万科 A  2017-06-20  21.03
2  2  4  国农科技  2017-06-20  27.03
3  3  5  世纪星源  2017-06-20  5.45
4  4  6  深振业 A  2017-06-20  8.87
5  5  7  全新好  2017-06-20  15.87
```

使用 map 函数映射一个字典即可：

```
In [70]: b={1:'银行',2:'房地产',4:'医药生物',5:'房地产',6:'采掘',7:'休闲服务',8:'机械设备'}
In [71]: a['ind']=a.ticker.map(b)
In [72]: a
Out[72]:
Unnamed: 0  ticker  secShortName  tradeDate  closePrice  ind
0  0  1  平安银行  2017-06-20  9.12  银行
1  1  2  万科 A  2017-06-20  21.03  房地产
2  2  4  国农科技  2017-06-20  27.03  医药生物
3  3  5  世纪星源  2017-06-20  5.45  房地产
4  4  6  深振业 A  2017-06-20  8.87  采掘
5  5  7  全新好  2017-06-20  15.87  休闲服务
```