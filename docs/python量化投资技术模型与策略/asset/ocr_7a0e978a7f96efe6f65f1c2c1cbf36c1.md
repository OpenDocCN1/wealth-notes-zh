![](bbox=[0.21, 0.09, 0.79, 0.27])

图 7-4

将s_year输出到csv文件，示例代码如下：

```
s_year.to_csv('s_year.csv')
```

csv文件的数据格式如图7-5所示。

我们也可以将年份和季度的位置调换，这样更符合日常使用习惯。但调换之前需要先将s_year转换成dataframe类。示例代码如下：

```
df_year=s_year.to_frame()
df_year.swaplevel(1,2,axis=0)
```

最后得到的数据如图7-6所示。