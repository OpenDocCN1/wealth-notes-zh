```
from collections import deque
import numpy as np
import talib as ta
import matplotlib.pyplot as plt
from CAL.PyCAL import *
NI1601 = DataAPI.MktFutmGet(ticker=u"NI1601",beginDate=u"",endDate=u"",field=u"endDate,turnoverVol",pandas="1").sort_values(by='endDate')
NI1701 = DataAPI.MktFutmGet(ticker=u"NI1701",beginDate=u"",endDate=u"",field=u"endDate,turnoverVol",pandas="1").sort_values(by='endDate')
NI = NI1601.append(NI1701)
NI.drop_duplicates(subset='endDate', keep='first', inplace=True)
NI = NI.set_index('endDate')
fig = plt.figure(figsize=(10, 6))
ax = fig.add_subplot(111)
ax.set_axisbelow('True')
ax = NI.plot(kind='bar', ax=ax, color='r', legend=False)
s = ax.set_title(u'沪镍1月交割月成交量', fontproperties=font, fontsize=16)
```

![](bbox=[0.18, 0.435, 0.82, 0.785])

在套利时，我们需要计算均衡价差，一般有两种算法。

+   ○ 模拟交割法：通过模拟交割过程中的各项费用，来计算套利过程中的各项成本，从而得出均衡价差。优点为容易计算，价差变化小；缺点为在实际情况下价差很