# 从零开始学 Python 大数据与量化交易

![](bbox=[0.1, 0.13, 0.925, 0.235])

图 13.9 EXPMA 指标函数示例的运行结果

# 13.2.9 VMA 指标函数

VMA 指标即变异平均线。变异平均线 VMA 指标的语法格式如下：

```
VMA(security_list, check_date, timeperiod = 12)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。

参数 timeperiod 表示统计的天数。

返回 VMA 的值，返回类型也与 MACD 指标相同，这里不再多说。

变异平均线 VMA 指标用法如下。

+   （1）股价高于平均线，视为强势；股价低于平均线，视为弱势。
（2）平均线向上 reclamo，具有助涨力道；平均线向下下跌，具有助跌力道。
（3）两条以上平均线向上交叉时，买进。
（4）两条以上平均线向下交叉时，卖出。

> 提醒 VMA 比一般平均线的敏感度更高，消除了部分平均线落后的缺陷。

VMA 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“VMA 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
# 定义股票池列表，调用 get_concept_stocks 函数，获取风力发电概念板块的成分股
security_list1 = get_concept_stocks('GN036')
# 输出 security_list1 的 12 日变异平均线值
VMA1 = VMA(security_list1, check_date='2018-08-01', timeperiod=12)
for stock in security_list1:
    print(VMA1[stock])
```