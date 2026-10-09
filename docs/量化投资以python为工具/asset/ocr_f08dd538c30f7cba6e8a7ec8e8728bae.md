# 22.2 Python 中的时间序列数据

图 22.1 上证综指收盘指数时序图是用 Python 画出的，下面我们介绍画出时序图的具体 Python 代码。

```
In [1]: import pandas as pd

In [2]: Index=pd.read_table('TRD_Index.txt',sep='\t',index_col='Trddt')

In [3]: SHIndex=Index[Index.Indexcd==1] #查看前3期数据
In [4]: SHIndex.head(n=3)
Out[4]:
```

| | Indexcd | Daywk | Opnindex | Hiindex | Loindeax | CIsindex | Retindeax |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Trddt | | | | | | | |
| 2014/1/2 | 1 | 4 | 2112.126 | 2113.110 | 2101.016 | 2109.387 | -0.003115 |
| 2014/1/3 | 1 | 5 | 2101.542 | 2102.167 | 2075.899 | 2083.136 | -0.012445 |
| 2014/1/6 | 1 | 1 | 2078.684 | 2078.684 | 2034.006 | 2045.709 | -0.017967 |

首先我们读取上证综指数据。查看上证综指的数据前 3 行，可以看到该数据的一些基本变量信息。各变量的含义简述如下。

+   - Indexcd: 指数代码 (Index code)，000001 表示上证综合指数；
- Trddt: 交易日期 (Trading date)，以 YYYY-MM-DD 表示；
- Daywk: 星期，1= 星期一，2= 星期二，3= 星期三，4= 星期四，5= 星期五，6= 星期六，0= 星期天；
- Opnindex: 开盘指数 (Open index)，每日交易中的第一条指数；
- Hiindex: 最高指数 (Highest index)，每日交易中的最高一条指数；
- Loindeax: 最低指数 (Lowest index)，每日交易中的最低一条指数；
- CIsindex: 收盘指数 (Closed index)，每日交易中的最后一条指数；
- Retindeax: 指数收益率 (Index return)。

```
# 查看数据SHindex的类型
In [5]: type(SHIndex)
Out[5]: pandas.core.frame.DataFrame
# 提取上证综指的收盘指数数据
In [6]: CIsindex=SHindex.CIsindex

In [7]: CIsindex.head(n=3)
Out[7]:
```

| Trddt | | 
| :---: | :--- |
| 2014/1/2 | 2109.387 |
| 2014/1/3 | 2083.136 |
| 2014/1/6 | 2045.709 |
| Name: CIsindex, dtype: float64 | |

```
In [8]: type(Clsindex)
Out[8]: pandas.core.series.Series

In [9]: type(Clsindex.index)
```