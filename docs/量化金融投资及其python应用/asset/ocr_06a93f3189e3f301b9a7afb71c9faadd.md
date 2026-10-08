```
```python
from scipy import stats
import statsmodels.api as sm            # 统计相关的库
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
IndexData = DataAPI.MktIdxGet(indexID = u"", ticker = u"000001",beginDate = u"20130101",
endDate = u"20140801",field =u"tradeDate,closeIndex,CHGFct", pandas = "i")
IndexData = IndexData.set_index(IndexData['tradeDate'])
data = np.array(IndexData['CHGFct'])                 # 上证指数日涨跌
IndexData['CHGFct'].plot(figsize = (15,5))
```
生成的图形如图 7-8 所示。
```

![](bbox=[0.15, 0.3, 0.85, 0.48])

可以看出，序列看上去是弱平稳的。下面画出序列的 ACF：

```
```python
fig = plt.figure(figsize= (20,5))
ax1 = fig.add_subplot(111)
fig = sm.graphics.tsa.plot_acf(data,ax=ax1)
```
可以发现，图 7-9 所示的 ACF 在 43 处截尾，之后的 ACF 均在置信区间内，由此可以判定该序列的 MA 模型阶次为 43。
```

![](bbox=[0.15, 0.65, 0.85, 0.8])

# ### 7.3.3 建模和预测
由于 sm.tsa 中没有单独的 MA 模块，因此利用 ARMA 模块，只要将其中 AR 的阶 $p$ 设为 0 即可。