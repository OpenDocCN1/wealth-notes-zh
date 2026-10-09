K 线图和 OHLC 数据看起来简单，但具体编程非常烦琐，常用的绘图模块如 Plotly、Matplotlib 等都集成了 OHLC 数据和 K 线图的绘制函数，大家直接调用即可。

# 案例 2-2：绘制金融数据 K 线图

案例 2-2 文件名为 kc202_zd_dr.py，介绍如何使用最新的 Plotly 绘图模块，绘制基于 Web 的动态交互式 K 线图。

案例 2-2 程序很简单，下面我们采用分组的模式进行讲解。首先讲解程序头的 import 部分代码。

```
```
import pandas as pd
import plotly as py
import plotly.figure_factory as pyff
#from plotly.tools import FigureFactory as pyff

import zsys
import ztools as zt
import ztools.str as zstr
import ztools_draw as zdfr
import zpd_talib as zta
```
```

在 import 部分代码中，需要注意的是：

```
```
import plotly.figure_factory as pyff
#from plotly.tools import FigureFactory as pyff
```
```

上面代码使用的是 Plotly 2.0 版本以后的新语法，其中被注释掉的代码是老版本的格式。这可能是因为在实际编程当中，plotly.figure_factory 模块使用得较多，新版本特意将它从 plotly.tools 模块中抽取出来，以简化编程代码。

接下来的几条 import 语句，用于导入 TopQuant 极宽量化系统中的模块。zwPython 已经集成到 Python 开发环境当中，对于其他的 Python 平台，特别是 Mac、Linux 平台的用户，在使用前请把 TopQuant 源码路径加入开发平台搜索路径当中，或者把 TopQuant 源码全部放入当前项目的工作目录下，然后使用 import 即可直接导入。