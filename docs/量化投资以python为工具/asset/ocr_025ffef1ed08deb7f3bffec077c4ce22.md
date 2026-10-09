![](bbox=[0.05, 0.08, 0.86, 0.43])

# 30.2.2 加权移动平均函数

用 Python 编写一个加权移动平均的函数 wmaCal( )，以实现数据的加权移动平均。wmaCal( ) 函数定义代码如下：

```
In [1]: def wmaCal(tsPrice,weight):
         import pandas as pd
         import numpy as np
         k=len(weight)
         arrWeight=np.array(weight)
         Wma=pd.Series(0.0,index=tsPrice.index)
         for i in range(k-1,len(tsPrice.index)):
             Wma[i]=sum(arrWeight*tsPrice[(i-k+1):i+1])
         return(Wma)
```

用青岛啤酒股票的日收盘价数据来说明一下 wmaCal( ) 函数的使用方法。

```
In [2]: wma5=wmaCal(Close,v)
         wma5.head()
Out[2]:
Date
2014-01-02    0.000000
2014-01-03    0.000000
2014-01-06    0.000000
2014-01-07    0.000000
2014-01-08    46.778667
dtype: float64
```

```
In [3]: wma5_weight=wmaCal(Close,[0.1,0.15,0.2,0.25,0.3])
```