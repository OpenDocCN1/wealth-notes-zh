```
print('\\n#2,dateLst:',zsys.dateLst)
print('\\ndf.tail()')
print(df.tail())
```

在以上代码中，使用的是极宝 ztools_str 模块库中的 str_2xtim 函数，其实底层使用的是新一代 Python 时间模块库 Arrow。

第2组代码对应的输出信息如图3-7所示。

![](bbox=[0.15, 0.3, 0.85, 0.41])

由以上输出信息可以看出，通过调用 str_2xtim 函数，我们把单一的时间数据拆分成更加独立的数据，便于统计分析。

因为这里使用的是日线数据，所以拆分后的数据与 zsys 模块预定义的 dateLst 一致。

```
dateLst: ['xyear', 'xmonth', 'xday', 'xday_week', 'xday_year', 'xweek_year']
```

其中各参数说明如下：

+   * xyear，年代。
* xmonth，月份。
* xday，日期。
* xday_week，周儿，注意周日返回值是0。
* xday_year，每年第几日。
* xweek_year，每年第几周。

以上参数还可以扩展，比如扩展成季度等。

第3组代码如下：

```
#3 计算时间衍生参数，使用 ztools_data 模块库中的函数
df= pd.read_csv(fss, index_col=0)
df=df.sort_index(ascending=True);
```