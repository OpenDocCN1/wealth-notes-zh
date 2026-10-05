![](bbox=[0.26, 0.135, 0.69, 0.345])

图 5-7 累计盈亏曲线

# 5.1.3 vn.py 回测

（1）导入包。

pymongo 用于在 Python 环境中连接到 MongoDB 数据库，VtBarData 里保存了 K 线数据，DAILY_DB_NAME 是 vn.py 定义的 MongoDB 数据库存放日线数据的地方。

```
In[1]
#加载模块
%matplotlib inline
import tushare as ts
import pymongo                    #MongoDB 的 Python 客户端
from datetime import datetime

from vnpy.trader.vtObject import VtBarData  # vtObject 里面保存了所有交易平台用到的
                                            #数据
from vnpy.trader.app.ctaStrategy.ctaBase import DAILY_DB_NAME  #加载 vn.py 数据库
                                                        #常量
```

（2）获取数据。

ts.get_hist_data 通过 Tushare 把行情数据导入内存中。Sort_index()则可对时间序列进行重新排序，其顺序是从最老的数据到最新的数据，因为 Tushare 默认下