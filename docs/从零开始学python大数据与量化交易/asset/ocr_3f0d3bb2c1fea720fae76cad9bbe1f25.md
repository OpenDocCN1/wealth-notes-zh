# 第13章 Python 量化择时的技术指标函数

（4）MFI<20，而产生背离现象时（价格在下跌，但资金流量指数在上涨），视为买进信号。

MFI 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“MFI 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqlib.technical_analysis import *
# 定义股票池列表
security_list1 = ['000001.XSHE','000002.XSHE','000009.XSHG']
# 计算并输出 security_list1 的 MFI 值
MFI1 = MFI(security_list1,check_date='2018-08-01', timeperiod=14)
print("平安银行（000001）、万科 A（000002）、中国宝安（000009）的资金流量具体如下： ")
for stock in security_list1:
    print(MFI1[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.12 所示。

![](bbox=[0.13, 0.45, 0.84, 0.58])

# 13.3.3 RSI 指标函数

RSI 指标即相对强弱指标。在 Python 量化交易策略中，相对强弱指标 RSI 的语法格式如下：

```
RSI(security_list, check_date, N1=6)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。

参数 N1 表示统计的天数。

返回 RSI 的值，返回类型也与 MACD 指标相同，这里不再多说。

相对强弱指标 RSI 用法如下。

+   （1）RSI>80 为超买，RSI<20 为超卖。

+   （2）RSI 以 50 为中界限，大于 50 视为多头行情，小于 50 视为空头行情。