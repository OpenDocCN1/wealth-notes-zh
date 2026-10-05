# 11.2.2 TuShare的Python SDK

TuShare是一个免费的数据接口项目，可用于提取数据。需要注意的是，TuShare的版本正在不断迭代，使用方法请以官网介绍为主，这里只做一个简单介绍。

导入TuShare，命令如下：

```
import tushare as ts
```

这里需要注意的是，TuShare的版本需要大于1.2.10。

设置token，命令如下：

```
ts.set_token('your token here')
```

以上方法只需要在第一次或者token失效后调用，即可完成调取TuShare数据凭证的设置，正常情况下不需要重复设置。也可以忽略此步骤，直接用pro_api('your token')语句完成初始化。

初始化pro接口的命令如下：

```
pro = ts.pro_api()
```