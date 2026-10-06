创建 DataFrame 示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择 “我的策略” → “投资研究” 命令，打开 “投资研究” 页面。双击 Pandas 文件夹，然后单击 “新建” 按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为 “创建 DataFrame”，然后输入以下代码：

```
import pandas as pd
data = {"name":["yahoo","google","facebook"], "marks":[200,400,800], "price":[9, 3, 7]}
dataframe1 = pd.DataFrame(data)
print("显示 dataframe1 中的数据：")
print(dataframe1)
```

单击工具栏中的 运行 按钮，运行结果如图 8.7 所示。

![](bbox=[0.143, 0.374, 0.815, 0.467])

# 8.3.2 数据的查看

下面利用 get_price() 函数获得股票数据，得到的数据是 DataFrame 对象。

get_price() 函数的语法格式如下：

```
get_price(security, start_date=None, end_date=None, frequency='daily', fields=None, skip_paused=False, fq='pre', count=None)
```

get_price() 函数可以按天或者按分钟获取一只或者多只股票的行情数据，各项参数意义如下。

security: 一只股票代码或者一只股票代码的 list。

start_date: 开始时间，与参数 count 二选一，不可同时使用。需要注意的是，如果参数 count 和参数 start_date 都没有设置，则 start_date 生效值是 2015-01-01。如果取分钟数据时，时间可以精确到分钟，如传入 datetime.datetime(2015, 1, 1, 10, 0, 0) 或者 '2015-01-01 10:00:00'。

> 提醒 当取分钟数据时，如果只传入日期，则日内时间是当日的 00:00:00。当取天数据时，则传入的日内时间会被忽略。