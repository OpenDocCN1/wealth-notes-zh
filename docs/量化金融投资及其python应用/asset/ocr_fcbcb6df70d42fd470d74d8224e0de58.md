```
import numpy as np # 数值计算
import statsmodels.api as sm # 统计运算
import scipy.stats as scs # 科学计算
import matplotlib.pyplot as plt # 绘图
```

# 1. 选择股票代号、获取股票数据、清理及可视化

```
symbols = ['hs300', '600000', '000980', '000981']
# 把相应股票的收盘价按照日期顺序存入 DataFrame 对象中
data = pd.DataFrame()

hs300_data = ts.get_hist_data('hs300', '2016-01-01', '2016-12-31')
hs300_data = hs300_data['close'] # 取沪深 300 指数收盘价数据
hs300_data = hs300_data[::-1] # 按日期从小到大排序
data['hs300'] = hs300_data

data1 = ts.get_hist_data('600000', '2016-01-01', '2016-12-31')
data1 = data1['close'] # 浦发银行股票收盘价数据
data1 = data1[::-1]
data['600000'] = data1

data2 = ts.get_hist_data('000980', '2016-01-01', '2016-12-31')
data2 = data2['close'] # 金圣股份收盘价数据
data2 = data2[::-1]
data['000980'] = data2

data3 = ts.get_hist_data('000981', '2016-01-01', '2016-12-31')
data3 = data3['close'] # 银亿股份收盘价数据
data3 = data3[::-1]
data['000981'] = data3

# 数据清理
data = data.dropna()
data.head()

# 规范化后的时序数据
(data/data.ix[0] * 100).plot(figsize = (8, 4))
```

运行上面的代码，得到如图 20-4 所示的图形。

![](bbox=[0.23, 0.64, 0.76, 0.87])