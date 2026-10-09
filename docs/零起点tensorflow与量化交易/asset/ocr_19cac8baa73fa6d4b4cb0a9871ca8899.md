```
```python
hdr,fss='K线图-'+xcod,‘tmp/tmp_.html’
df2=df.tail(100)
zdr.drDF_cd1(df2,ftg=fss,m_title=hdr)
```
```

绘制最近 100 组数据的 K 线图，如图 2-7 所示。

![](bbox=[0.16, 0.21, 0.87, 0.54])

# 案例 2-6：TopDat 金融数据集的 Tick 数据

案例 2-6 文件名是 kc206_ztick.py，主要介绍如何调用 TopDat 金融数据集的 Tick 数据，并绘制价格曲线图。

本案例需要下载 Tick 数据包，完整的 Tick 数据包解压后约为 160GB。

下面我们分组对案例程序进行讲解。

第 1 组代码如下，进行初始化处理。

```
```python
#1 预处理
pd.set_option('display.width', 450)
pyplt=py.offline.plot
```
```

第 2 组代码如下：

```
```python
#2
xcod='600663'
```
```