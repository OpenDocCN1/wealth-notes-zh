Seaborn 模块被用于在 Python 中制作有吸引力的统计图形，它构建在 Matplotlib 之上，支持 NumPy 和 Pandas 的数据结构，以及来自 SciPy 和 StatsModels 的统计结果。该模块旨在用尽可能少的参数生成尽可能漂亮的图形，不过 Seaborn 是对 Matplotlib 的补充，而不是替代品。

Seaborn 的主要功能如下。

+   - 通过内置主题改善 Matplotlib 的外观。\n- 有丰富的调色板工具，可更好地显示数据。\n- 对变量分布进行可视化。\n- 数据矩阵可视化，并使用聚类算法发现这些矩阵中的结构。\n- 对自变量和因变量之间的线性回归结果进行可视化。\n- 绘制统计时间序列，并将其不确定性可视化。\n- 构建高级、抽象的网格图，可轻松地将复杂的问题可视化。

# 3.5.1 主题管理

Seaborn 的一个重要优点是内置了经过美化的主题，这类似于我们经常使用的 PPT 模板，用户无须在外观调整上花费多余的精力。我们通过一个简单的例子进行理解。

首先，导入 NumPy 及 Matplotlib 模块：

```
In [147]:import numpy as np
    import matplotlib as mpl
    import matplotlib.pyplot as plt
    %matplotlib inline
In [148]:plt.rcParams['font.sans-serif']=['SimHei']   #用来正常显示中文标签
    plt.rcParams['axes.unicode_minus']=False   #用来正常显示负号
```

然后，定义一个函数，用来绘画正弦函数，全部使用 Matplotlib 中的默认参数，不需要任何调整：

```
In [149]:def sinplot(flip=1):
        x = np.linspace(0, 14, 100)
        for i in range(1, 7):
            plt.plot(x, np.sin(x + i * .5) * (7 - i) * flip)
In [150]:sinplot()
```