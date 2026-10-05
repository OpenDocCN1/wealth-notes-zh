# （2）ADF 值

ADF 值是推论统计学的概念。通过 ADF 检验来得出其 ADF 值，然后与 10% 进行比较，若 ADF 值大于 10%，则证明完全不能拒绝原假设，即原假设成立。原假设如有单位根则代表着品种自相关性强，具有趋势行情。

总结一下，增加了两个初步筛选的标准，分别是：调整后波动率比值>1；ADF 值>10%。

代码实现如下。

```
import Numpy as np
import pandas as pd
import statsmodels.tsa.stattools as ts

def volPriceTick (SYM, PT):
    c=pymongo.MongoClient()
    symbol = SYM        #合约代号
    col = c['VnTrader_Daily_Db'][symbol]
    cx = col.find()
    l = list(cx)
    d = {}
    for key in l[0].keys():
        d[key] = []
    for data in l:
        for k, v in data.items():
            d[k].append(v)
    df = pd.DataFrame.from_dict(d)
    PriceTick = PT      #最小价格变动
    close = df['close']
    var = np.std(close)                 #收盘价的标准差
    ratio = var / (PriceTick*100)       # 定义调整后波动率比值
    adfresult = ts.adfuller(close)      # 得到 ADF 值
    return ratio, adfresult
```

调用定义好的函数进行测试，得出调整后的波动率比率和 ADF 值如图 6-17 所示，品种是沪深 300 股指期货（IF99），其最小价格变动是 0.02。