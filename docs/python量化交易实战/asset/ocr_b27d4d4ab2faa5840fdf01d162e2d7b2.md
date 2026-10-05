在显示时并没有什么问题，但是在后续计算时，由于 nan 并不是一个具体的数值，有很大可能报错。

这里 NumPy 提供了相应的函数对其进行处理，代码如下：

```
ma3 = np.nan_to_num(ma3)
```

具体结果请读者自行验证。

而对于不同滑动平均值的计算，通过设置不同的 timeperiod 值进行针对性设置。

可能有读者提出疑问，在介绍 history_n 函数时，着重介绍了其中的参数设置，其中的参数之一 frequency 是对函数获取的标的时间周期进行设置，此时在【程序 5-2】中可以通过获取不同的 frequency 对序列时间进行设置。

# 【程序 5-2】

```
from gm.api import *
import talib
import numpy as np

set_token("9e02621e7fcf850731def2e8a9de1fdbd5b21ad6")

data =
history_n(symbol="SZSE.399006",frequency="3600s",count=100,end_time="2017-12-31",fields="close",fill_missing="last",adjust=ADJUST_PREV,df=True)
close = np.asarray(data["close"].values)
ma5 = talib.MA(close,timeperiod = 5)
ma5 = np.nan_to_num(ma5)
print(ma5)
```

通过不同 timeperiod 可以获取不同的时间端数据。然而无论获取的数据多少，其都是基于统一的 “1d”，也就是以日为时间周期去获取。而有时读者在进行程序设计的时候需要使用不同的时间周期去进行处理，此时则只需要将 frequency 设置成

```
frequency="3600s"
```

即可。

# 5.1.2 EMA 的计算

EMA 的中文意思是指数平均数指标，也叫 EXPMA 指标，是一种趋向类指标，是以指数式递减加权的移动平均。

EMA 的公式如下：

EMA = α × Price_N + (1 - α) × Price_(N-1)

其中，α 为平滑指数，一般取作 2/(N+1)。