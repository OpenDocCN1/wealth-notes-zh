# 8.1 history()函数的运用技巧

history()函数可以查看一只股票或多只股票的历史行情数据，其语法格式如下：

```
history(count, unit='1d', field='avg', security_list=None, df=True, skip_paused=False, fq='pre')
```

需要注意的是，获取的天数据，不包括当天的 数据，即使是在收盘后。

关于停牌：因为获取了多只股票的数据，其中可能有的股票停牌，有的股票没有停牌，为了保持时间轴一致，默认没有跳过停牌的日期，停牌时使用停牌前的数据填充。

# 8.1.1 各项参数的意义

history()函数共有 7 项参数，各项参数意义如下。

1) count

参数 count，表示数量，即返回的结果集的行数。

2) unit

参数 unit，表示单位时间长度，几天或者几分钟，即只支持 “Xd” 和 “Xm”，其中，X 是一个正整数；“Xd”表示具体几天，如 10d，表示单位时间长度为 10 天；“Xm”表示具体几分钟，如 10m，表示单位时间长度为 10 分钟。

需要注意的是，当 X > 1 时，fields 只支持 “open” “close” “high” “low” “volume” “money” 这几个标准字段。

其中，open 表示时间段股票开始时的价格；close 表示时间段股票结束时的价格；high 表示时间段股票的最高价格；low 表示时间段股票的最低价格；volume 表示时间段股票的成交量；money 表示时间段股票的成交金额。

3) field

参数 field，表示获取的数据类型，支持 SecurityUnitData 里面所有的基本属性，包括 “open” “close”“low”“high”“volume”“money”“factor”“high_limit”“low_limit”“avg”“pre_close” “paused”。

前面 6 个字段的意义同参数 unit，这里不再赘述，下面为其他字段的意义。

factor：前复权因子。