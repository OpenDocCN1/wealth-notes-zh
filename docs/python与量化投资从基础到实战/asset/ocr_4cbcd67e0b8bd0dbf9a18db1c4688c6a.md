# 峰值低点示例图

![](bbox=[0.18, 0.13, 0.77, 0.38])

所以，高低点突破策略的具体逻辑如下。

+   ◎ 前收盘价格 $pre_{closeprice}$ 向上突破动态峰值高点，形成做多信号，买入开仓。
◎ 前收盘价格 $pre_{closeprice}$ 向下突破动态峰值低点，形成做空信号，卖出开仓。

策略代码如下：

```
#***************************************************#
# 峰值高低点突破 —— 无止盈止损
# 价格上穿峰值高点，形成做多信号，买入开仓
# 价格下穿峰值高点，形成做空信号，卖出开仓
#***************************************************#
import talib
import pandas as pd
import numpy as np

### 参数初始化
universe = ['RB1610']          # 策略证券池
start = pd.datetime(2016, 6, 1)   # 回测开始时间
end  = pd.datetime(2016, 9, 1)   # 回测结束时间
capital_base = 1e5             # 初试可用资金
refresh_rate = 1               # 调仓周期
freq = 'd'                     # 调仓频率: s-> 秒; m-> 分钟; d-> 日;

## 自动生成保证金比例: margin_rate
margin_ratio = DataAPI.FutuGet(ticker = universe, field =
```