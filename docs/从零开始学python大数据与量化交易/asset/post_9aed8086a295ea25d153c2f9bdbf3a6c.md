
# 从零开始学 Python 大数据与量化交易

DMA(security_list, check_date, N1 = 10, N2 = 50, M = 10)

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。
参数 N1、N2 和 M 表示统计的天数。
返回 DIF 和 DIFMA 的值，返回类型也与 MACD 指标相同，这里不再多说。
平均差指标 DMA 用法具体如下。

- （1）DMA 向上交叉其平均线时，买进。
- （2）DMA 向下交叉其平均线时，卖出。
- （3）DMA 的交叉信号比 MACD 略快。
- （4）DMA 与股价产生背离时的交叉信号，可信度较高。
- （5）DMA 和 MACD 两者构成一组指标，互相验证。

DMA 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“DMA 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqlib.technical_analysis import *
# 定义股票池列表
security_list1 = '000001.XSHE'
security_list2 = ['000001.XSHE','000002.XSHE','601211.XSHG','600001.XSHG']
# 计算并输出 security_list1 的 DMA 值
DIF1,DIFMA1 = DMA(security_list1,check_date='2017-01-04',N1 = 10, N2 = 50, M = 10)
print(DIF1[security_list1])
print(DIFMA1[security_list1])
# 输出 security_list2 的 DMA 值
DIF2,DIFMA2 = DMA(security_list2,check_date='2017-01-04', N1 = 10, N2 = 50, M = 10)
for stock in security_list2:
    print(DIF2[stock])
    print(DIFMA2[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.6 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_294_0.png)

图 13.6 DMA 指标函数示例的运行结果

## 13.2.6 JS 指标函数

JS 指标，即加速线。加速线 JS 指标的语法格式如下：

```
JS(security_list, check_date, N = 5, M1 = 5, M2 = 10, M3 = 20)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。

参数 N、M1、M2、M3 表示统计的天数。

返回 JS、MAJS1、MAJS2 和 MAJS3 的值，返回类型也与 MACD 指标相同，这里不再多说。

加速线指标是衡量股价涨速的工具，加速线指标上升表明股价上升动力增加，加速线指标下降表明股价下降压力增加。

加速线适用于 DMI 表明趋势明显时（DMI.ADX > 20）使用。

- (1) 如果加速线在 0 值附近形成平台，则表明既不是最好的买入时机也不是最好的卖出时机。
- (2) 在加速线发生金叉后，均线形成底部是买入时机。
- (3) 在加速线发生死叉后，均线形成顶部是卖出时机。

JS 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“JS 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
# 定义股票池列表
security_list1 = ['000001.XSHE','000002.XSHE','601211.XSHG']
# 输出 security_list2 的 JS 值
js_jsx, js_majsx1, js_majsx2, js_majsx3 = JS(security_list1,check_date='2018-08-01', N = 5, M1 = 5, M2 = 10, M3 = 20)
for stock in security_list1:
    print(js_jsx[stock])
    print(js_majsx1[stock])
    print(js_majsx2[stock])
    print(js_majsx3[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.7 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_296_0.png)

图 13.7 JS 指标函数示例的运行结果

## 13.2.7 MA 指标函数

MA 指标即均线。均线 MA 指标的语法格式如下：

```
MA(security_list, check_date, timeperiod=5)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。参数 timeperiod 表示统计的天数。返回 MA 的值，返回类型也与 MACD 指标相同，这里不再多说。均线 MA 指标用法如下。

- （1）股价高于平均线，视为强势；股价低于平均线，视为弱势。
- （2）平均线向上涨升，具有助涨力道；平均线向下下跌，具有助跌力道。
- （3）两条以上平均线向上交叉时，买进。
- （4）两条以上平均线向下交叉时，卖出。
- （5）移动平均线的信号经常落后股价，若以 EXPMA、VMA 辅助，可以改善。

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

# 第 13 章 Python 量化择时的技术指标函数

单击工具栏中的 运行 按钮，运行结果如图 13.8 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_297_0.png)

## 13.2.8 EXPMA 指标函数

EXPMA 指标即指数平均线。指数平均线 EXPMA 指标的语法格式如下：

```
EXPMA(security_list, check_date, timeperiod = 12)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。参数 timeperiod 表示统计的天数。返回 EXPMA 的值，返回类型也与 MACD 指标相同，这里不再多说。指数平均线 EXPMA 指标用法如下。

- （1）EXPMA 一般以观察 12 日和 50 日两条均线为主。
- （2）12 日指数平均线向上交叉 50 日指数平均线时，买进。
- （3）12 日指数平均线向下交叉 50 日指数平均线时，卖出。

EXPMA 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“EXPMA 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
security_list1 = ['000001.XSHE','000002.XSHE','601211.XSHG']
# 输出 security_list2 的 EXPMA 值
EXPMA1 = EXPMA(security_list1,check_date='2018-08-01', timeperiod=12)
for stock in security_list1:
    print(EXPMA1[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.9 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_298_0.png)

图 13.9 EXPMA 指标函数示例的运行结果

## 13.2.9 VMA 指标函数

VMA 指标即变异平均线。变异平均线 VMA 指标的语法格式如下：

```
VMA(security_list, check_date, timeperiod = 12)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。

参数 timeperiod 表示统计的天数。

返回 VMA 的值，返回类型也与 MACD 指标相同，这里不再多说。

变异平均线 VMA 指标用法如下。

- （1）股价高于平均线，视为强势；股价低于平均线，视为弱势。
- （2）平均线向上上扬，具有助涨力道；平均线向下下跌，具有助跌力道。
- （3）两条以上平均线向上交叉时，买进。
- （4）两条以上平均线向下交叉时，卖出。

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

单击工具栏中的运行按钮，运行结果如图 13.10 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_299_0.png)

## 13.3 反趋向指标函数

反趋向指标，又称超买超卖型技术指标。对某种股票的过度买入称为超买；反之，对于某种股票的过度卖出则称为超卖。反趋向指标属于分析大势的一种技术分析指标，它分析的主要用途在于大势的涨跌气势，使股市潜在的趋势清楚地表现出来。

### 13.3.1 KD 指标函数

KD 指标即随机指标。随机指标 KD 的语法格式如下：

```
KD(security_list, check_date, N =9, M1=3, M2=3)
```

各参数意义如下。

- security_list: 股票列表，可以是一只股票，也可是多只股票。
- check_date: 要查询数据的日期。
- N: 统计的天数 N。
- M1: 统计的天数 M1。
- M2: 统计的天数 M2。

随机指标 KD 的返回值是 K 和 D 的值，是一个字典(dict)类型，其中键(key)为股票代码，值(value)为数据。

随机指标 KD 用法如下。

- 1. 指标大于 80 时，回档概率大；指标小于 20 时，反弹概率大。
- (2) K 在 20 左右向上交叉 D 时，视为买进信号。
- (3) K 在 80 左右向下交叉 D 时，视为卖出信号。

> 提醒 KD 波动于 50 左右的任何信号，其作用不大。

KD 指标函数示例如下。

双击“量化择时”文件夹，单击“新建”按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为“KD 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
# 定义股票池列表
security_list1 = '000001.XSHE'
# 计算并输出 security_list1 的 KD 值
K1,D1 = KD(security_list1, check_date = '2018-08-01', N = 9, M1 = 3, M2 = 3)
print("KDJ 指标的 K 值: ",K1[security_list1])
print("KDJ 指标的 D 值: ",D1[security_list1])
```

单击工具栏中的 运行 按钮，运行结果如图 13.11 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_300_0.png)

### 13.3.2 MFI 指标函数

MFI 指标即资金流量指标。在 Python 量化交易策略中，资金流量指标 MFI 的语法格式如下：

```
MFI(security_list, check_date, timeperiod=14)
```

各参数意义与 MACD 指标相同，这里不再多说。返回值是一个字典(dict)类型，也与 MACD 指标相同，这里不再多说。

资金流量指标 MFI 用法如下。

- (1) MFI>80 为超买，当其回头向下跌破 80 时，为短线卖出时机。
- (2) MFI<20 为超卖，当其回头向上突破 20 时，为短线买进时机。
- (3) MFI>80，而产生背离现象时（价格在上涨，但资金流量指数在下跌），视为卖出信号。

# 第13章 Python 量化择时的技术指标函数

- （4）MFI<20，而产生背离现象时（价格在下跌，但资金流量指数在上涨），视为买进信号。

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

![](img/261a0e647c121c3aa8af17bcacd0d7ae_301_0.png)

### 13.3.3 RSI 指标函数

RSI 指标即相对强弱指标。在 Python 量化交易策略中，相对强弱指标 RSI 的语法格式如下：

```
RSI(security_list, check_date, N1=6)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。

参数 N1 表示统计的天数。

返回 RSI 的值，返回类型也与 MACD 指标相同，这里不再多说。

相对强弱指标 RSI 用法如下。

- （1）RSI>80 为超买，RSI<20 为超卖。
- （2）RSI 以 50 为中界限，大于 50 视为多头行情，小于 50 视为空头行情。
- （3）RSI 在 80 以上形成 M 头或头肩顶形态时，视为向下反转信号。
- （4）RSI 在 20 以下形成 W 底或头肩底形态时，视为向上反转信号。
- （5）RSI 向上突破其高点连线时，买进；RSI 向下跌破其低点连线时，卖出。

RSI 指标函数示例如下。

双击 “量化择时” 文件夹，单击 “新建” 按钮，在弹出的下拉菜单中选择 Python3 命令，新建一个 Python3 文件，并命名为 “RSI 指标函数”，然后输入以下代码：

```
#导入 technical_analysis 库
from jqllib.technical_analysis import *
# 定义股票池列表
security_list1 = ['000001.XSHE','000002.XSHE','000009.XSHG']
# 计算并输出 security_list1 的 RSI 值
RSI1 = RSI(security_list1, check_date='2018-08-01', N1=6)
print("平安银行（000001）、万科 A（000002）、中国宝安（000009）的 RSI 值具体如下： ")
for stock in security_list1:
    print(RSI1[stock])
```

单击工具栏中的 运行 按钮，运行结果如图 13.13 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_302_0.png)

### 13.3.4 OSC 指标函数

OSC 指标即变动速率线。在 Python 量化交易策略中，变动速率线 OSC 的语法格式如下：

```
OSC(security_list, check_date, N = 20, M = 6)
```

其中，security_list 和 check_date 参数与 MACD 指标相同，这里不再多说。
参数 N 和 M 表示统计的天数 N 和统计的天数 M。
返回 OSC 和 MAOSC 的值，返回类型也与 MACD 指标相同，这里不再多说。
变动速率线 OSC 用法如下。
