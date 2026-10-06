# 第 9 章 Python 大数据可视化的 Matplotlib 包

![](bbox=[0.153, 0.084, 0.805, 0.237])

# 图 9.1 matplotlib 文件夹

双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“默认的绘图对象”，然后输入以下代码：

```
import numpy as np
from matplotlib import pyplot as plt
x = np.arange(1,25)
y =  6 * x +  11
plt.figure()
plt.plot(x,y)
plt.show()
```

首先导入 numpy 包，并设置别名为 np；然后导入 matplotlib 中的 pyplot 模块，并设置别名为 plt。接着定义变量 x 为一个数组，变量 y 为变量 x 的一次方程。然后调用 figure() 绘制对象，接着在对象中绘制图形，最后显示绘制对象。

单击工具栏中的 运行 按钮，运行结果如图 9.2 所示。

![](bbox=[0.157, 0.556, 0.797, 0.807])

# 图 9.2 默认的绘图对象示例的运行结果

含有参数的绘图对象示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投