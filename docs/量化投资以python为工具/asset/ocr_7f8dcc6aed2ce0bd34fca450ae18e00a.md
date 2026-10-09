```
```
In [1]: from datetime import datetime

In [2]: import pandas as pd

In [3]: date = datetime(2016,1,1)
In [4]: date = pd.Timestamp(date)
In [5]: date
Out[5]: Timestamp('2016-01-01 00:00:00')
In [6]: type(date)
Out[6]: pandas._tslib.Timestamp
In [7]: ts=pd.Series(1,index=[date])
In [8]: ts
Out[8]:
2016-01-01	 1
dtype: int64
In [9]: ts.index
Out[9]: DatetimeIndex(['2016-01-01'], dtype='datetime64[ns]', freq=None)
In [10]: ts.index[0]
Out[10]: Timestamp('2016-01-01 00:00:00')
```
```

不过, 由于 Timestamp() 并不接受列表等可迭代对象, 如果我们想要创建一个 Series 对象, 则需要对每个日期应用一次 Timestamp() 函数, 这样显然很麻烦。因此, 我们一般会使用 to_datetime() 将 Series 的 index 属性转换为 DatetimeIndex:

```
```
In [11]: dates = ['2016-01-01','2016-01-02', '2016-01-03']
In [12]: ts=pd.Series([1,2,3],index=pd.to_datetime(dates))
In [13]: ts
Out[13]:
2016-01-01	 1
2016-01-02	 2
2016-01-03	 3
dtype: int64
In [14]: ts.index
Out[14]: DatetimeIndex(['2016-01-01', '2016-01-02', '2016-01-03'],
 dtype='datetime64[ns]', freq=None)
In [15]: ts.index[0]
Out[15]: Timestamp('2016-01-01 00:00:00')
```
```

实际上, 对于 datetime 对象, 我们可以直接将其作为 index, 而 Pandas 会自动将其转换成 Timestamp 对象: