# 12.4.6 人工神经网络在量化交易中的运用实例

下面利用神经网络机器算法，预测中国石化（600028）2018年8月28日的涨跌行情，然后再将预测结果与实际情况进行对比，判断预测对错。

双击“Python量化交易策略的机器算法运用技巧”文件夹，然后单击“新建”→“Python 3”命令，新建一个Python 3 文件，并将其命名为“神经网络在量化交易中的应用”。

首先导入需要的数据包，具体代码如下：

```
import talib                #导入talib库
from jqdata import *        #导入聚宽函数库
#从pybrain的数据集中导入分类数据集
from pybrain.datasets import ClassificationDataSet
```

接下来设置要操作的股票，即利用神经网络训练的股票。同时要设置训练股票的开始时间和结束时间，具体代码如下：

```
test_stock = '600028.XSHG'      #设置测试标的为中国石化
start_date = datetime.date(2013, 9, 6)    #设置开始时间
end_date = datetime.date(2018, 8, 28)     #设置结束时间
```

接下来，利用 get_all_trade_days()函数获取所有交易日；再定义两个变量，将其分别赋值为神经网络训练开始时间和结束时间，具体代码如下：

```
trading_days = list(get_all_trade_days())           #获取所有交易日
start_date_index = trading_days.index(start_date)      #获取开始时间
end_date_index = trading_days.index(end_date)          #获取结束时间
```

接下来定义两个分类数据集，分别用来训练数据和测试预测数据，具体代码如下：

```
#训练数据，2018年8月28日之前所有的日期的数据都用来训练
#这是一个分类数据集，参数有3个，分别是输入的特征的数目、输出的结果的数目、类别的数目
#输入的特征的数目有3个，输出的结果只有一个，其类别是涨或者跌，所以参数是3,1,2
trndata = ClassificationDataSet(3, 1, nb_classes=3)
#测试数据，2018年8月28日的数据用来测试预测
tstdata = ClassificationDataSet(3, 1, nb_classes=3)
```

接着利用 for 循环语句计算 3 个指标，即简单移动平均线指标、加权移动平均数指标和动量线指标的数据，并添加到列表变量中，具体代码如下：

```
for index in range(start_date_index, end_date_index):
    #得到计算指标的所有数据
    start_day = trading_days[index - 30]
    end_day = trading_days[index]
```