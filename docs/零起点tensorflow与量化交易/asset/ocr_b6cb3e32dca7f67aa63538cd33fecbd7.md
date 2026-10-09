# 案例 3-3：时间因子

案例 3-3 文件名是 ke303_tim01.py，主要介绍如何拆分时间数据，把单一的时间数据扩展成更加专业的数据集。换句话说，就是扩展更加专业的时间因子。

第 1 组代码如下：

```
#1 读取数据
fss='data/600663.csv'
print('\n#1,fss',fss)
df=pd.read_csv(fss,index_col=0)
df=df.sort_index(ascending=True);
print(df.tail())
```

读取数据，并按日期排序，其对应的输出信息是：

```
#1, fss data/600663.csv

      open  high  low  close  volume

date

2017-09-04  24.26  24.40  24.20  24.27  25418.0

2017-09-05  24.20  24.28  23.91  24.00  35635.0

2017-09-06  24.00  23.88  23.64  23.71  38661.0

2017-09-07  23.70  24.02  23.70  23.84  24343.0

2017-09-08  23.72  24.00  23.68  23.76  23190.0
```

第 2 组代码如下：

```
#2 计算时间衍生参数
#

df['xtim']=df.index

df['xyear']=df['xtim'].apply(zstr.str_2xtim,ksgn='y')

df['xmonth']=df['xtim'].apply(zstr.str_2xtim,ksgn='m')

df['xdav']=df['xtim'].apply(zstr.str_2xtim,ksgn='d')
#

df['xdav_week']=df['xtim'].apply(zstr.str_2xtim,ksgn='dw')

df['xdav_year']=df['xtim'].apply(zstr.str_2xtim,ksgn='dy')

df['xdav_month']=df['xtim'].apply(zstr.str_2xtim,ksgn='dm')

df['xweek_year']=df['xtim'].apply(zstr.str_2xtim,ksgn='wy')
```