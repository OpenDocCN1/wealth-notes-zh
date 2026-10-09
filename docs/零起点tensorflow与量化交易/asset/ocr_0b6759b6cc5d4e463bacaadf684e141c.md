本案例中为了防止覆盖 TopData 数据，使用了临时目录 tmp 来保存数据文件。以下代码用于删除实时数据目录下的所有文件，以免长期运行，占用更多的系统空间。

```
`zt.f_dirDel(rss)`
```

第 1 组代码对应的输出信息是：

```
# 删除实时数据文件
for file in zt.f_dirList(rss):
    zt.f_dirDel(file)
```

```text
rss#1: /zDat/real/
rss#2: tmp/
t 2017-09-13 5
```

第 2 组代码如下，用于设置股票代码，下载实时数据，并绘制对应的 K 线图。

```
```python
#2 下载股票实时数据文件
xcode = '603316'
df = zddown.down_min_real010(rss, xcode, xtype=xtyp, fgIndex=False)
print('\\nxcode:', xcode)
print(df.tail())

hdr, fss = 'K线图-' + xcode, 'tmp/tmp_' + xcode + '.html'
df.index = df['date']
zdr.drDF_cdl(df, ftd=fss, m_title=hdr)
```
```

因为是下载股票数据，`down_min_real010` 函数中的 `fgIndex` 变量参数赋值为 False。

第 2 组代码对应的输出信息是：

```
tmp/603316.csv, 2017-09-13
xcode: 603316
```

| | date | open | high | low | close | volume |
|---|---|---|---|---|---|---|
| 525 | 2017-09-13 | 14:40 | 17.28 | 17.28 | 17.24 | 17.25 | 685.0 |
| 526 | 2017-09-13 | 14:45 | 17.25 | 17.25 | 17.16 | 17.20 | 1109.0 |
| 527 | 2017-09-13 | 14:50 | 17.20 | 17.22 | 17.19 | 17.22 | 365.0 |
| 528 | 2017-09-13 | 14:55 | 17.22 | 17.26 | 17.21 | 17.24 | 837.0 |
| 529 | 2017-09-13 | 15:00 | 17.25 | 17.29 | 17.25 | 17.27 | 1243.0 |

对应的 K 线图如图 2-12 所示。