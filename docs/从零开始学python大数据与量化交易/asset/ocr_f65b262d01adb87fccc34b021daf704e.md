在这里，建立 subplot 网格，为 2 行 2 列，然后调用 plot()函数绘制图形。另外，这里还调用 title()函数为图形添加标题。

单击工具栏中的 运行 按钮，运行结果如图 9.6 所示。

![](bbox=[0.188, 0.162, 0.782, 0.379])

# 9.5 add_axes 方法的应用

add_axes 方法为新增子区域，该区域可以坐落在 figure 内任意位置，且该区域可任意设置大小。下面通过实例来讲解。

add_axes 方法的应用示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择 “我的策略” → “投资研究” 命令，打开 “投资研究” 页面。双击 matplotlib 文件夹，然后单击 “新建” 按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为 “add_axes 方法的应用”，然后输入以下代码：

```
import numpy as np
import matplotlib.pyplot as plt
#新建 figure
fig = plt.figure()
# 定义数据
x = [1, 2, 3, 4, 5, 6, 7]
y = [1, 3, 4, 2, 5, 8, 6]
#新建区域 ax1
#figure 的百分比,从 figure 10%的位置开始绘制,宽高是 figure 的 80%
left, bottom, width, height = 0.1, 0.1, 0.8, 0.8
# 获得绘制的句柄
ax1 = fig.add_axes([left, bottom, width, height])
ax1.plot(x, y, 'r')
ax1.set_title('area1')
```