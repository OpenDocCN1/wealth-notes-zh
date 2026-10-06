# 第 13 章 Python 量化择时的技术指标函数

（1）OSC 以 100 为中轴线，OSC>100 为多头市场；OSC<100 为空头市场。

（2）OSC 向上交叉其平均线时买进；OSC 向下交叉其平均线时卖出。

（3）OSC 在高水平或低水平与股价产生背离时，应注意股价随时有反转的可能。

> 提醒 OSC 的超买超卖界限值随个股不同而不同，投资者应自行调整。

OSC 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“OSC 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
# 定义股票池列表，调用 get_industry_stocks 函数，获取计算机/互联网行业的成分股
security_list1 = get_industry_stocks('164')
# 计算并输出 security_list1 的 OSC 值
OSC1, MAOSC1 = OSC(security_list1, check_date = '2018-08-01', N = 20, M = 6)
print("计算机/互联网行业的成分股的 OSC 值具体如下：")
for stock in security_list1:
    print(OSC1[stock],"        ",MAOSC1[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.14 所示。

![](bbox=[0.13, 0.53, 0.84, 0.75])

# 图 13.14 OSC 指标函数示例的运行结果

# 13.3.5 WR 指标函数

WR 指标即威廉指标。在 Python 量化交易策略中，威廉指标 WR 的语法格式如下：

```
WR(security_list, check_date, N = 10, N1 = 6)
```