```
import numpy
import talib
import pandas as pd
close = numpy.random.random(100)
ema = talib.EMA(close,5)
```

生成的数据如图12-2所示。

| Index | close | ema |
|---|---|---|
| 0 | 0.60 | nan |
| 1 | 0.31 | nan |
| 2 | 0.39 | nan |
| 3 | 0.07 | nan |
| 4 | 0.53 | 0.38 |
| 5 | 0.79 | 0.51 |
| 6 | 0.87 | 0.63 |
| 7 | 0.72 | 0.66 |
| 8 | 0.37 | 0.56 |
| 9 | 0.11 | 0.41 |
| 10 | 0.29 | 0.37 |
| 11 | 0.63 | 0.46 |
| 12 | 0.86 | 0.59 |