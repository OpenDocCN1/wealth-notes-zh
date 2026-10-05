# Python 机器学习与量化投资

357.73 357.73 1.87

1on-DF: 12095 1on-DF: 12093

第 2 段代码如下：

```
02
02.1
print('\n2# train')
df2 pd.DataFrame()
df2['jpy']=jpy['Close']
df2['eur']=eur['Close']
zt.pDF('12.1 edf2',df2)

02.2
df2['jpy'] df2['jpy']/100
zt.pDF('02.2 @df2.x',df2)
```

把 USDJPY（美元/日元交易对）、USDEUR（美元/欧元交易对）两组不同的汇率数据，合并到一个数据表 df2 中以便于比较。

注意 2.2 段的代码：

```
df2['jpy']=df2['jpy']/100
```

把日元的价格缩小以便于两者比较，这里的 100 是经验参数，不是正规的数据归一化处理。

第 3 段代码如下：

```
03
df3=df2.head(300)
df3 df3.sort_index()
zdr.drr_line(df3,'USDJPY v USDEUR 汇率对比曲线',xlet=['jpy','eur'])
```

绘制汇率曲线图，这段代码使用了几个小技巧。

技巧1，数据选取：

```
df3=df2.head(300)
```