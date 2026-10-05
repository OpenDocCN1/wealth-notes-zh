# 10.9.3 ARMA模型阶次判定

一般来说，判断p要使用PACF函数，判断q要使用ACF函数，这里可以同时绘制PACF和ACF图形，示例代码如下：

```
import tushare as ts
import pandas as pd
import numpy as np
import statsmodels.api as sm
import matplotlib.pyplot as plt
%matplotlib inline

df=ts.get_k_data('399300', index=True,start='2015-01-01',
end='2015-12-31')
df=df.set_index('date')
df['rtn']=np.log(df['close'])-np.log(df['close'].shift(1))
df=df.dropna()

fig = plt.figure(figsize=(10,8))
ax1=fig.add_subplot(211)
fig = sm.graphics.tsa.plot_acf(df.rtn,lags=30,ax=ax1)
ax2 = fig.add_subplot(212)
fig = sm.graphics.tsa.plot_pacf(df.rtn,lags=30,ax=ax2)
```

得到的结果如图10-14所示。