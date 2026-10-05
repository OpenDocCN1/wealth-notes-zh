```
[ 'SHSE.600000', 'SHSE.600016', 'SHSE.600019', 'SHSE.600028', 'SHSE.600029', 'SHSE.600030', 'SHSE.600036', 'SHSE.600048', 'SHSE.600050', 'SHSE.600104', 'SHSE.600111', 'SHSE.600309', 'SHSE.600340', 'SHSE.600518', 'SHSE.600519', 'SHSE.600547', 'SHSE.600606', 'SHSE.600837', 'SHSE.600887', 'SHSE.600919', 'SHSE.600958', 'SHSE.600999', 'SHSE.601006', 'SHSE.601088', 'SHSE.601166', 'SHSE.601169', 'SHSE.601186', 'SHSE.601211', 'SHSE.601229', 'SHSE.601288', 'SHSE.601318', 'SHSE.601328', 'SHSE.601336', 'SHSE.601398', 'SHSE.601601', 'SHSE.601628', 'SHSE.601668', 'SHSE.601669', 'SHSE.601688', 'SHSE.601766', 'SHSE.601800', 'SHSE.601818', 'SHSE.601857', 'SHSE.601878', 'SHSE.601881', 'SHSE.601985', 'SHSE.601988', 'SHSE.601989', 'SHSE.603993']
```

下面将选取的成分股与因子组合在一起，要求选取当日的上证 50 成分股的增长率。

# 【程序 6-1】

```
from gm.api import *
import datetime

set_token("715f84a04a4574e2e77491d62f7c8bbc0c663b12")
day_time,hour_and_mins=str(datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')).split(" "):

index = "SHSE.000016"

symbol_list=get_history_constituents(index=index,start_date=day_time)[0].get("constituents").keys()

df=
get_fundamentals_n(table="deriv_finance_indicator",symbols=symbol_list,end_date=day_time,
fields="NPGRT",count=1,df=True)

print(df.head(5))
```

【程序 6-1】中首先导入了掘金量化的程序包，而 datetime 是获取当前时间的辅助包，可以通过函数获取当前系统对应的时间并以字符串的形式打印出来，之后通过字符串分割出当前的年月日和时分秒。而 symbol_list 是获取当前日期上证 50 构成的成分股，之后将