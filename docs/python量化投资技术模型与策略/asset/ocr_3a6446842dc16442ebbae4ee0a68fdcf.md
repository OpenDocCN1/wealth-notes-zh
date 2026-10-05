返回值：
	df: 进行复权处理后的数据, OHLC所有的价格数据都要进行复权, 并返回对应的复权因子adj_factor

```
'''
# 生成前一天的收盘价, 以便于向量化计算复权价格
df['close_pre']=df.close.shift(1)

# 判断是否换月
df['roll_over']=
(df.trade_hiscode!=df.trade_hiscode.shift(1)) & (~df.trade_hiscode.shift(1).isnull())

# 初始化复权因子
df['adj_factor']=0

# 计算因换月问题导致的复权因子变化
df.loc[df.roll_over,'adj_factor']=df.loc[df.roll_over,'close_pre']-df.loc[df.roll_over,'open']

# 累积所有复权因子的变化, 得到最终的复权因子
df['adj_factor_cum']=df.adj_factor.cumsum()

# 对OHLC的四个数据都进行复权
df['open']=df.open + df.adj_factor_cum
df['high']=df.high + df.adj_factor_cum
df['low']=df.low + df.adj_factor_cum
df['close']=df.close + df.adj_factor_cum

# 删除中间数据
del df['close_pre']
del df['roll_over']
del df['adj_factor']
del df['adj_factor_cum']

df=df.dropna()

return df
```

在以上函数中, 我们使用了Pandas的shift函数完成了今日和昨日的比较和运算, 使用cumsum()函数完成了复权因子的累加。