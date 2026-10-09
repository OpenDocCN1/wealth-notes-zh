![](bbox=[0.35, 0.08, 0.55, 0.2])

图 27.6 乌云盖顶与抱线

# 2. Python 捕捉上证综指 “乌云盖顶” 形态

以 “乌云盖顶” 形态为例，我们尝试运用 Python 捕捉上证综指 2011 年的 K 线图中的 “乌云盖顶” 形态。

# （1） 定义 “乌云盖顶” 形态

+   • 第 1 个（前一期）红色蜡烛实体：收盘价高于开盘价；第 2 个（当期）绿色蜡烛实体：收盘价低于开盘价；

+   • 第 2 天（当期）的开盘价高于第 1 天（前一期）的收盘价；第 2 天（当期）收盘价位于第 1 天（前一期）实体的下半部分范围内，即第 2 天（当期）收盘价低于第 1 天（前一期） 收盘价与开盘价之和的一半，且大于第 1 天（前一期）的开盘价；

+   • 定义前期上升趋势：连续两期的收益率为正。

# （2） Python 捕捉 “乌云盖顶” 形态

使用上证综指 2011 年的日交易数据，编写 Python 代码捕捉 “乌云盖顶” 形态。

```
#提取读入上证综指年的日交易数据
In [1]: import pandas as pd
In [2]: ssec2011=pd.read_csv('ssec2011.csv')
     ssec2011.index=ssec2011.iloc[:,1]
     ssec2011.index=pd.to_datetime(ssec2011.index,\
     format='ZY-%m-%d')
     ssec2011=ssec2011.iloc[:,2:] 

#提取价格数据
In [3]: Close11=ssec2011.Close
     Open11=ssec2011.Open

#判断捕捉符合 “乌云盖顶” 形态的连续两个蜡烛实体
In [4]: lagClose11=Close11.shift(1)
     lagOpen11=Open11.shift(1)
     Cloud=pd.Series(0,index=Close11.index)
     for i in range(1,len(Close11)):
     if all([Close11[i]<Open11[i],\
     lagClose11[i]>lagOpen11[i],\
     Open11[i]>lagClose11[i],\
     Close11[i]<0.5*(lagClose11[i]+lagOpen11[i]),\
     Close11[i]>lagOpen11[i]]):
     Cloud[i]=1
```