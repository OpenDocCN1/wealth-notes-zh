第 2 组代码对应的输出信息如下:

```
```
#2,df.tail()
       time price change volu me amount type
4 14:56:48 24.12 -- 13 31356 buy
3 14:56:51 24.13 0.01 7 16891 sell
2 14:56:54 24.11 -0.02 30 72330 sell
1 14:57:00 24.11 -- 0 0 sell
0 15:00:03 24.11 -- 241 581436 sell
```
```

注意其中的字段名称, 和前面介绍的 TuShare 中的 get_tick_data 函数完全一致, 这是因为本案例中的 Tick 数据来自 TopDat 金融数据集, 主要是使用 TuShare 抓取的。

第 3 组代码如下:

```
```
#3
print('\n#3, plot-->>tmp/tmp_.html')
html, fss='Tick数据价格曲线图*', 'tmp/tmp_.html'
df2=df.tail(200)
zdr.drDF_tickX(df2, ftag=fss, m_title=hdr, sgntim='time', sgnPrice='price')
```
```

绘制 Tick 数据的价格曲线图。为了简化代码, 本案例中没有直接使用 Ptdty 的绘图函数, 而是调用 TopQuamt 绘图模块中的 zdr.dr_tickX 函数。

此外, 为了使画面美观, 本案例只使用最后的 200 组数据进行绘图, 如图 2-4 所示。

![](bbox=[0.15, 0.67, 0.85, 0.85])