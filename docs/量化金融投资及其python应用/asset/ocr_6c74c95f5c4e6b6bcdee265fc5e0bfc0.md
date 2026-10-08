| 时 期 | 股 票 1 | 股 票 2 | 债 券 |
| --- | --- | --- | --- |
| 12 | 0.33 | 0.99 | 0.11 |
| 13 | $ -0.05 $ | $-0.25$ | 0.15 |
| 14 | 0.22 | 0.04 | 0.11 |
| 15 | 0.23 | $-0.11$ | 0.09 |
| 16 | 0.06 | $-0.15$ | 0.10 |
| 17 | 0.32 | $-0.12$ | 0.08 |
| 18 | 0.19 | 0.16 | 0.06 |
| 19 | 0.05 | 0.22 | 0.05 |
| 20 | 0.17 | $-0.02$ | 0.07 |

求这 3 个资产的投资组合夏普比最大化和方差最小化的权数。

先用表 20-1 的数据在目录 G:\2glkx\data 下建立 tzsy.xls 数据文件。

```
#准备工作
import pandas as pd
import numpy as np                #数值计算
import statsmodels.api as sm     #统计计算
import scipy.stats as scs        #科学计算
import matplotlib.pyplot as plt  #绘制图形
```

# 1. 选取股票

代码如下：

```
#取数
data = pd.DataFrame()
data = pd.read_excel('G:\\2glkx\\data\\tzsy.xls')
data = pd.DataFrame(data)
#清理数据
data = data.dropna()
data.head()
data.plot(figsize = (8,3))
```

运行上面的代码，得到如图 20-1 所示的图形。

![](bbox=[0.262, 0.708, 0.735, 0.836])

图 20-1 3 个投资对象的收益率变化