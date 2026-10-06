# 从零开始学 Python 大数据与量化交易

![](bbox=[0.155, 0.115, 0.87, 0.305])

图 13.7 JS 指标函数示例的运行结果

# 13.2.7 MA 指标函数

MA 指标即均线。均线 MA 指标的语法格式如下：

```
MA(security_list, check_date, timeperiod=5)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。参数 timeperiod 表示统计的天数。返回 MA 的值，返回类型也与 MACD 指标相同，这里不再多说。均线 MA 指标用法如下。

+   （1）股价高于平均线，视为强势；股价低于平均线，视为弱势。
（2）平均线向上涨升，具有助涨力道；平均线向下下跌，具有助跌力道。
（3）两条以上平均线向上交叉时，买进。
（4）两条以上平均线向下交叉时，卖出。
（5）移动平均线的信号经常落后股价，若以 EXPMA、VMA 辅助，可以改善。

MA 指标函数实例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“MA 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
#定义股票池列表
security_list1 = '000001.XSHE'
# 计算并输出 security_list1 的 MA 值
MA1 = MA(security_list1, check_date='2018-08-01', timeperiod=5)
print("平安银行的 5 日均线为: ",MA1[security_list1])
```