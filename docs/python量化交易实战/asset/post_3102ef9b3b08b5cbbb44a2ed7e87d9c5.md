
![](img/66b32d04cb77b904cead314891984c93_54_0.png)

图 3-15 降水量的四分位图

从图 3-15 中可以直观地看到，不同月份之间的降水量有很大的差距，春秋（1~4 月）降水量明显较少，5 月份开始降水量有个明显增多的过程，而在 7 月份达到顶峰后回落，11 月份和 12 月份达到最低的降水量。

同时可以看到，有几个月份的降水量有明显的偏移，即离群值出现，这点可能跟年度情况有关，需要继续进行分析。

## 3.5.2 不同月份之间的增减程度比较

正常情况下，每年降水量都呈现一个平稳的增长或者减少的过程，其下降的坡度（趋势线）应该是一样的。【程序 3-14】展示了这种趋势。

### 【程序 3-14】

```
from pylab import *
import pandas as pd
import matplotlib.pyplot as plot
    filePath = ("c://rain.csv")
dataFile = pd.read_csv(filePath)

summary = dataFile.describe()
minRings = -1
maxRings = 99
nrows = 11
for i in range(nrows):
    dataRow = dataFile.iloc[i,1:13]
    labelColor = (dataFile.iloc[i,12] - minRings) / (maxRings - minRings)
    dataRow.plot(color=plot.cm.RdYlBu(labelColor), alpha=0.5)
```

```
plot.xlabel("Attribute")
plot.ylabel(("Score"))
show()
```

最终打印结果如图 3-16 所示。

![](img/66b32d04cb77b904cead314891984c93_55_0.png)

图 3-16 降水量的趋势图

从图 3-16 中可以明显地看到，降雨的月份并不是规律地上涨或下跌，而是呈现一个不规则的浮动状态，增长最快的为 6、7 月份，而下降最快的为 7、8 月份，之后有一个明显的回升过程。

## 3.5.3 每月的降水量是否相关

每月的降水量理论上来说应该是具有相互独立性的，即每月的降水量和其他月份没有关系。但是实际是这样的吗？

### 【程序 3-15】

```
from pylab import *
import pandas as pd
import matplotlib.pyplot as plot
    filePath = ("c:// rain.csv")
dataFile = pd.read_csv(filePath)

summary = dataFile.describe()
corMat = DataFrame(dataFile.iloc[1:20,1:20].corr())

plot.pcolor(corMat)
plot.show()
```

通过计算，最终结果如图 3-17 所示。

![](img/66b32d04cb77b904cead314891984c93_56_0.png)

图 3-17 月份之间的相关性显示

从图 3-17 可以看到，颜色分布比较平均，对应的区块相互之间没有太大的相关性，因此可以认为每月的降水量和其他月份没有关系，是独立行为。

## 3.6 小结

本章从直观的观察开始，逐渐深入介绍和研究了数据集和分析工具，了解了使用 Python 类库进行数据分析的基本方法。数据分析从最基本的矩阵转换开始，直到对数据集特征值进行分析和处理，通过对本章内容的 Lazarus，读者可以为学习数据分析打下基础。

使用相应的类库进行深度学习程序设计是本章的重点，也是希望读者能够掌握的内容。再一次强调，请读者尽量使用 Python 已有的类库进行程序设计。在数据的可视化展示过程中，通过做出多种数据图形向读者演示了使用不同的类库可以非常直观地进行数据分析，希望本章中提供的不同研究方法和程序设计思路能够帮助读者掌握基本数据集的描述性和统计值之间的关系，这些非常有利于对数据的掌握。

本章是数据处理的基础，内容简单，但是非常重要，希望读者能够使用不同的数据集进行处理并演示更多的值。

## 第 4 章

## ◄ 欢迎来到掘金量化 ►

欢迎来到掘金量化！

掘金量化交易平台（软件启动界面见图 4-1）涵盖量化交易完整的生命周期，支持多语言策略开发、tick 级回测、仿真交易与实盘交易，为客户提供安全、专业、高效的量化 IT 解决方案。

本章将介绍掘金量化平台的注册与使用。

![](img/66b32d04cb77b904cead314891984c93_57_0.png)

## 4.1 基础工作

本节将介绍使用掘金量化平台前需要做的各种准备工作，包括从开始注册到获取帮助。

### 4.1.1 安装掘金终端

在处理测量数据时，经常要研究变量之间的关系。变量之间的关系一般分为两种：
- 完全确定关系，即函数关系。
- 相关关系，即变量之间既存在着密切联系，但又不能由一个或多个变量的值求出另一个变量的值。

#### 1. 下载终端

登录掘金官网（http://www.myquant.cn）申请试用，申请通过后下载终端。完成后双击安装应用程序，进入安装向导，根据向导完成安装。

#### 2. 注册用户/登录用户

启动终端后，进入用户登录界面（见图 4-2），使用手机号为用户名进行注册并登录。

![](img/66b32d04cb77b904cead314891984c93_58_0.png)

图 4-2 登录掘金量化终端

#### 3. 安装对应的 SDK

进入终端后，单击左上角的 logo，如图 4-3 所示。

![](img/66b32d04cb77b904cead314891984c93_58_1.png)

图 4-3 单击登录 logo

在选择策略编程语言界面上选择自己熟悉的语言，进行相应 SDK 的安装，如图 4-4 所示。

![](img/66b32d04cb77b904cead314891984c93_58_2.png)

图 4-4 安装 SDK

之后进入掘金量化终端的首页，如图 4-5 所示。

![](img/66b32d04cb77b904cead314891984c93_59_0.png)

读者需要注意的是，在安装 SDK 之前，需确认已经安装有相应的编程语言环境，比如安装 Python 语言的 SDK 前，需要先安装 Python 2 或者 Python 3.6 的环境。这里建议安装 Python 3.6。

### 4.1.2 获取帮助

在掘金量化中，获取帮助一般分为两种：从网站直接获取帮助和通过 QQ 群获取帮助。这里建议读者先尝试自己从网站查找对应的 API 文档，遇到经过查询无法解决的问题时，再通过 QQ 群获取帮助。

- 1. 获取帮助

通过首页右上方的“帮助中心”获取帮助，如图 4-6 所示。

![](img/66b32d04cb77b904cead314891984c93_59_1.png)

- 2. 一些常用页面介绍

（1）依次单击“帮助中心”→“API 文档”→Python，在代码编写过程中，可通过该页面查阅基本 API 函数和一些重要概念的解释，如图 4-7 所示。

图 4-7 创建策略页面介绍

（2）依次单击“帮助中心”→“数据文档”，该页面提供详细的基本面数据和行情数据，如图 4-8 所示。

图 4-8 数据文档介绍

除此之外，掘金网站上有很多不同的页面能够提供不同的文档介绍和帮助，希望读者多多使用。对于一些无法解决的问题，还可以通过网站社区上提供的问答来解决。

Python
定时任务
以下的代码片段是一个非高频的例子，在每个交易日的14:50:00点执行买入200股的模拟订单：

```
# coding=utf-8
from __future__ import print_function, absolute_import, unicode_literals
from gm.api import *
def init(context):
    schedule(schedule_func=algo, date_rule='1d', time_rule='14:50:00')
def algo(context):
    # 买入200股卖出指令
    order_volume(symbol='SHSE.600000', volume=200, side=OrderSide_Buy, order_type=Ord
if __name__ == '__main__':
    run(strategy_id='strategy_1', filename='main.py', mode=MODE_BACKTEST, token='toke
    backtest_start_time='2016-05-17 13:00:00', backtest_end_time='2017-08-21 15:0
```

整个流程需要三步：
设置初始化函数 init, 使用 schedule 函数进行定时任务设置
匹配任务，到点会执行任务
执行策略

## 4.2 实战：使用掘金终端进行回测工作

下面开始使用掘金终端进行回测工作。使用掘金量化相比较其他平台的好处在于，可以使用 Python IDE 编辑器进行数据处理和编辑，并通过掘金终端获取回测结果。推荐使用 IDE 对数据进行编辑，而不是使用掘金终端对工作进行回测。

### 4.2.1 创建第一个策略

下面将带领读者创建第一个策略。

选择主界面中“策略研究”，进入策略研究页面，如图 4-9 所示。该页面有“新建策略”和“策略研究”两个菜单。

![](img/66b32d04cb77b904cead314891984c93_61_0.png)

单击“新建策略”菜单，这里提供了多个不同类型的策略模板，可选择最合适的模板开始编写，如果都不匹配，可单击“空策略”来构建全新的策略，如图 4-10 所示。

![](img/66b32d04cb77b904cead314891984c93_61_1.png)

选择所需要的策略类型，输入策略名之后，单击“确认”按钮，这时 analizy 出来一个策略 ID，如需在熟悉的 IDE 中编写代码，则可将此 ID 复制到所编写的代码中与此终端进行连接，方可进行后续操作，如图 4-11 所示。

![](img/66b32d04cb77b904cead314891984c93_62_0.png)

图 4-11 策略 ID

如果需要再次查看策略 ID 或者修改策略解析器路径等一系列策略环境配置，可以单击“设置”按钮进行设置，如图 4-12 所示。

![](img/66b32d04cb77b904cead314891984c93_62_1.png)

图 4-12 查看相关内容

这是使用掘金量化终端创建第一个策略的方法，更多的尝试希望读者自行测试完成。

### 4.2.2 运行回测

下面进行运行回测。

（1）单击“策略研究”，选择指定策略进入策略编写界面，如图 4-13 所示。

![](img/66b32d04cb77b904cead314891984c93_63_0.png)

（2）该页面是一个完整的 IDE 工作区，可对策略内容进行修改调试以及回测，若策略较复杂，则可以选择右上角的“分页”按钮对策略进行分段编辑，如图 4-14 所示。

![](img/66b32d04cb77b904cead314891984c93_63_1.png)

（3）策略修改完成之后，单击右上方的“回测参数”按钮可对策略基本数据参数进行调整（见图 4-15），随后单击“运行回测”按钮即可开始回测。

图 4-15 设置起始参数

### 4.2.3 查看回测结果

在研究策略页面单击该策略中“回测次数”，如图 4-16 所示。

在该策略的回测历史列表页面可对单一回测结果进行备注标识，不需要的回测可进行删除操作：

图 4-16 回测次数

进入对应的回测结果界面，如图 4-17 所示。

![](img/66b32d04cb77b904cead314891984c93_65_0.png)

图 4-17 进入回测结果页面

该页面展示了此策略回测的所有明细情况，包括收益概览、信号分析、交易明细、每日持仓和输出日志。可单击屏幕右上方的“结果下载”将回测结果保存在本地，如图 4-18 所示。

![](img/66b32d04cb77b904cead314891984c93_65_1.png)

图 4-18 保存回测结果

至此，查看回测结果介绍完毕，读者可以通过下载掘金 IDE 和安装 Python 类库来自行测试，这里主要以了解为主，具体的 моменту推荐 Python 专用 IDE，例如 Pycharm。

### 4.2.4 使用 PyCharm 进行回测

前面已经介绍过，PyCharm 是一种 Python IDE，带有一整套可以帮助用户在使用 Python 语言开发时提高效率的工具，比如调试、语法高亮、Project 管理、代码跳转、智能提示、自动完成、单元测试、版本控制。此外，该 IDE 提供了一些高级功能，以用于支持 Django 框架下的专业 Web 开发。

掘金量化支持并鼓励读者使用 PyCharm 进行量化的回测工作，在这里通过一个例子演示使用 PyCharm 进行回测的结果。

- 1. 创建一个新的 Python 文件

单击 File 菜单并选择 New Project，在弹出的菜单中选择 Pure Python（编译器会默认选择当前已安装的 Python 版本），如图 4-19 所示。

![](img/66b32d04cb77b904cead314891984c93_66_0.png)

图 4-19 新建一个 Python 工程

在名为 untitled 的新建项目里，右击它并选择 New→File 来新建文件，如图 4-20 所示。

![](img/66b32d04cb77b904cead314891984c93_66_1.png)

图 4-20 创建一个新文件

输入新的文件名，例如 HelloQuant，如图 4-21 所示。

![](img/66b32d04cb77b904cead314891984c93_66_2.png)

图 4-21 设置文件名

- 2. 从掘金量化终端中复制回测框架

在“策略研究”页面单击“新建策略”菜单，如图 4-22 所示。

图 4-22 新建策略

之后选择一个任务，这里建议选择定时任务，单击“确认”按钮进行创建。之后确定部分变为如图 4-23 所示的形态。

图 4-23 确定已创建的任务

此时单击“策略编辑”按钮对策略进行编辑处理。这里生成的是一个具有完整框架的回测程序，如图 4-24 所示。

```
# coding=utf-8
from __future__ import print_function, absolute_import

from gm.api import *

def init(context):
    # 每天14:50 定时执行algo任务
    schedule(schedule_func=algo, date_rule='daily', time_rule='14:50:00')

def algo(context):
    # 购买200股浦发银行股票
    order_volume(symbol='SHSE.600000', volume=200, side=1,
                 order_type=2, position_effect=1, price=0)

# 查看最终的回测结果
def on_backtest_finished(context, indicator):
    print(indicator)

if __name__ == '__main__':
    run(strategy_id='36b05b5f-8fe6-11e8-a628-4cedfb681747',
        filename='main.py',
        mode=MODE_BACKTEST,
        token='e8978d765c4822e5a85fcaa73e044065cf17b58b',
        backtest_start_time='2016-06-17 13:00:00',
        backtest_end_time='2017-08-21 15:00:00')
```

图 4-24 创建策略文件

### 【程序 4-1】

```
# coding=utf-8
from __future__ import print_function, absolute_import

from gm.api import *

def init(context):
    # 每天14:50 定时执行algo任务
    schedule(schedule_func=algo, date_rule='daily', time_rule='14:50:00')

def algo(context):
    # 购买200股浦发银行股票
    order_volume(symbol='SHSE.600000', volume=200, side=1,
                 order_type=2, position_effect=1, price=0)
# 查看最终的回测结果
def on_backtest_finished(context, indicator):
    print(indicator)
if __name__ == '__main__':
    run(strategy_id='36b05b5f-8fe6-11e8-a628-4cedfb681747',
        filename='main.py',
        mode=MODE_BACKTEST,
        token='e8978d765c4822e5a85fcaa73e044065cf17b58b',
        backtest_start_time='2016-06-17 13:00:00',
        backtest_end_time='2017-08-21 15:00:00')
```

可以将以上代码段复制到 PyCharm IDE 中直接使用，与正常的掘金 IDE 一样。

> 注意 对于不同的文件名，需要将文件名改成 PyCharm 中 Python 的名称，即将 filename='main.py'改成 filename='HelloQuant.py'，这点请读者切记。
