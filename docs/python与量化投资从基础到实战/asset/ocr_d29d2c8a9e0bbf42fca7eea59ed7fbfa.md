的走势无关，即使如此，也取得了 7.5%的 Alpha。试想，如果在实盘中策略表现得依旧如此，则完全可以通过加大杠杆来获取更高的收益，的确是最赚钱的策略之一。

# 6.5.2 均值回复策略

前面已经介绍过均值回复策略，可将其主要分为跨品种套利及跨期套利。

# 1. 跨品种套利

跨品种套利所选择的品种从经济角度来说，可能处于同一产业链的上下游，比如玉米与淀粉；也有可能具有可替代或者互补的关系，比如豆油与菜油。一般来说，同一产业链的品种相关性较高，无论是从基本面还是从统计规律出发，均比较容易找到逻辑支撑。常见的产业链主要包括黑色产业链、豆类油脂产业链和化工产业链等。通过对同一产业链中相关性很强的品种的期货合约分别进行买入和卖出，通过品种间的强弱变化引发价差的收缩与扩大，可实现价差收益。“虚拟钢厂”的构建正是利用螺纹钢和其主要原材料铁矿石、焦炭及三种上下游品种之间的价格的不平衡变化而产生的价差收益，是一种跨品种套利的策略。

我们先来看看这三个品种的历史走势：

```
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
import talib
from collections import deque
from CAL.PyCAL import font
import DataAPI
import scipy.stats as st
rbm0 = DataAPI.MktMFutdGet(mainCon=u"1", contractObject=u"RB",startDate=u"2013-11-01",endDate=u"2016-06-01",field=[u'closePrice', 'tradeDate'],pandas="1")
im0 = DataAPI.MktMFutdGet(mainCon=u"1", contractObject=u"I",startDate=u"2013-11-01",endDate=u"2016-06-01",field=[u'closePrice', 'tradeDate'],pandas="1")
jm0 = DataAPI.MktMFutdGet(mainCon=u"1", contractObject=u"J",startDate=u"2013-11-01",endDate=u"2016-06-01",field=[u'closePrice', 'tradeDate'],pandas="1")
y = np.array(rbm0['closePrice'])
```