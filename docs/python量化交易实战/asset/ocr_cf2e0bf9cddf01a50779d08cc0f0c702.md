```
df["GPG"] = -999
df["OPG"] = -999
df["OCG"] = -999
```

> 意
在掘金数据集中，MPG 数据被 TAG 代替，这里只需要拟立 TAG 代码段即可。

可能读者注意到，在这里其值被定义为 -999，这样做的目的是，在读取数据时，为了防止有数据缺失或者出错，将默认值设置成现实数据中可能会遇到的最小值。这样做的好处是在后续计算时可以自动将出错的数据的计算结果降为最差结果。

# 2. 因子的获取

对于需要使用的因子，掘金量化也提供了对应的基本面获取函数：

```
def get_fundamentals_n(table, symbols, end_date, fields=None, filter=None,order_by=None, count=1, df=False):
```

其中，table 是对应的表名，field 是字段名，order_by 是排序方式，如果有一些需要过滤的字段，可以使用 filter 进行字段过滤。

首先看本模型中的第二个因子——NPG。通过查表可得，第一个需要查询的字段 NPG 在掘金数据库中的字段名为 NPGRT，其在 deriv_finance_indicator 表中，因此根据 get_fundamentals_n 函数可以写成如下形式：

```
get_fundamentals(table='deriv_finance_indicator', symbols=symbol_list, start_date=day_time, end_date=day_time, fields="NPGRT", df=True)
```

需要注意的是，对于部分数据有可能会产生缺失，因此在读取时需要对数据的数量进行判断，如果是正常读取，就 ASSERT? 就可以正常结束，而当数据出现缺失时，可使用默认的最小值进行填充，代码如下：

```
_df = get_fundamentals(table='deriv_finance_indicator', symbols=symbol_list, start_date=day_time, end_date=day_time, fields="NPGRT", df=True)
if len(_df) == len(symbol_list):
df["NPG"] = _df["NPGRT"]
else:
for number in range(len(symbol_list)):
try:
_df = get_fundamentals(table='deriv_finance_indicator', symbols=symbol_list[number], start_date=day_time, end_date=day_time, fields="NPGRT")
_NPG = tools.get_data_value(_df, "NPGRT")
df.iloc[number, 2] = _NPG[0]
except:
df.iloc[number, 2] = -999
```