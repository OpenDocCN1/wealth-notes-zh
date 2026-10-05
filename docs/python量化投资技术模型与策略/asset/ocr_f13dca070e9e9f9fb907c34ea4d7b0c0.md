# 10.9.1 MA模型

MA(q)模型的表达公式如下：

r_t = c_0 + a_t - θ_1 a_{t-1} - ... - θ_q a_{t-q}  (10-2)

其中，c_0是一个常数，{a_t}是一个白噪声序列。

Python并未为MA模型专门提供模型，如果需要拟合MA模型，只需要使用ARMA模型，将p置为0即可。这里演示一个简单的建模过程。

先使用ACF函数判断阶次q，程序代码如下：

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
df[' rtn']=np.log(df['close'])-np.log(df['close'].shift(1))
df=df.dropna()

fig = plt.figure(figsize=(10,5))
ax1=fig.add_subplot(111)
fig = sm.graphics.tsa.plot_acf(df.rtn,ax=ax1,lags=60)
```