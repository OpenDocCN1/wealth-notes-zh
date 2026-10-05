图11-6所示的是搜索出来的部分指标值。我们选择指标 NY.GDP.PCAP.PP.KD，即“GDP per capita,PPP(constant 2011 internation...)”作为人均生产总值的代表。

下面再搜索包含关键词cell的指标，命令如下：

```
wb.search('cell.*').iloc[:, :2]
```

得到的结果如图11-7所示。

![](bbox=[0.14, 0.41, 0.87, 0.57])

图 11-7

我们选择IT.MOB.COV.ZS作为手机覆盖率的代表。

下面就来获取数据（以2011年的数据为代表），命令如下：

```
ind = ['NY.GDP.PCAP.KD', 'IT.MOB.COV.ZS']
dat = wb.download(indicator=ind, country='all', start=2011,
end=2011).dropna()
dat.columns = ['gdp', 'cellphone']
print(len(dat))
print(dat.head())
```