济上利用外汇统计失效通常不像其他资产类别那样复杂，比如 VIX 波动率指数等波动率产品。外汇也经常提供免费和全面的数据。以下数据集来自 Refinitiv Eikon Data API。数据集已通过 API 读取，包含开盘价、最高价、最低价和收盘价，图 7-1 显示了收盘价。

```
In [1]: import os
import numpy as np
import pandas as pd
from pylab import pyplot, mpl
mpl.style.use('seaborn')
mpl.rcParams['savefig.dpi'] = 300
mpl.rcParams['font.family'] = 'serif'
pd.set_option('precision', 4)
np.set_printoptions(suppress=True, precision=4)
os.environ['PYTHONHASHSEED'] = '0'
```

```
In [2]: url = 'http://hilpisch.com/aif_etkon_id_eur_usd.csv'
```

```
In [3]: symbol = 'EUR_USD'
```

```
In [4]: raw = pd.read_csv(url, index_col=0, parse_dates=True)
```

```
In [5]: raw.head()
Out[5]:
       HIGH   LOW   OPEN  CLOSE
Date
2019-10-01 00:00:00  1.0899  1.0897  1.0897  1.0899
2019-10-01 00:01:00  1.0899  1.0896  1.0899  1.0898
2019-10-01 00:02:00  1.0898  1.0896  1.0898  1.0896
2019-10-01 00:03:00  1.0898  1.0896  1.0897  1.0898
2019-10-01 00:04:00  1.0898  1.0896  1.0897  1.0898
```

```
In [6]: raw.info()
<class 'pandas.core.frame.DataFrame'>
DateTimeIndex: 96526 entries, 2019-10-01 00:00:00 to 2019-12-31 23:00:00
Data columns (total 4 columns):
# Column  Non-Null Count  Dtype
0  HIGH    96526 non-null   float64
1  LOW     96526 non-null   float64
2  OPEN    96526 non-null   float64
3  CLOSE   96526 non-null   float64
dtypes: float64(4)
memory usage: 3.7 MB
```

```
In [7]: data = pd.DataFrame(raw['CLOSE'], loc=[])
data.columns = [symbol]
```

```
In [8]: data = data.resample('1h', label='right').last().ffill()
```

```
In [9]: data.info()
<class 'pandas.core.frame.DataFrame'>
DateTimeIndex: 2208 entries, 2019-10-01 01:00:00 to 2020-01-01 00:00:00
Freq: H
Data columns (total 1 columns):
```