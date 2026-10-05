```
df['old_pos']=df['pos']-df['new_pos']
```

假设我们都能以开盘价成交，即开仓价就是开盘价open，代码如下：

```
# 把开盘价作为交易价格
df['entry_p']=df['open']
```

分别计算两种仓位的盈亏值，代码如下：

```
# 计算旧仓位的盈利和新仓位的盈利
df['p&l_new']=(df['close']-df['entry_p'])*df['new_pos']
df['p&l_old']=(df['close']-df['close'].shift(1))*df['old_pos']

# 每日盈亏由两部分组成
df['p&l']=df['p&l_new']+df['p&l_old']
```

将每日的盈亏累加在一起，再加上初始资本，就可以看到资本曲线（如图13-2所示），代码如下：

```
# 计算累计盈亏
df['p&l_cum']=df['p&l'].cumsum()

# 计算净值曲线，假设初始资金是1000\aini_cap=1000
df['capital']=df['p&l_cum']+ini_cap
df['net_value']=df['capital']/ini_cap

# 绘制净值曲线
df=df.set_index('date')
df.plot(figsize=(12,6),y=['net_value'])
```