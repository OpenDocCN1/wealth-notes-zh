# 从零开始学 Python 大数据与量化交易

单击工具栏中的 ▶ 运行 按钮，运行结果如图 9.10 所示。

![](bbox=[0.12, 0.1, 0.91, 0.38])

图 9.10 设置线条的宽度和颜色示例的运行结果

# 9.9 坐标轴网格

用坐标轴对象中的 grid 可以使用和取消网格线；也可以用 plot 函数中同样的关键字参数来定制网格样式。

坐标轴网格示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择 “我的策略” → “投资研究” 命令，打开 “投资研究” 页面。双击 matplotlib 文件夹，然后单击 “新建” 按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为 “坐标轴网格”，然后输入以下代码：

```
import matplotlib.pyplot as plt
x = np.arange(1,25)
fig, axes = plt.subplots(1, 2, figsize=(10,3))
# 默认网格外观
axes[0].plot(x, x**1.2, x, x**2, lw=2)
axes[0].grid(True)
# 用户定义的网格外观
axes[1].plot(x, x**2, x, x**2.3, lw=3)
axes[1].grid(color='b', alpha=0.5, linestyle='dashed', linewidth=0.5)
```

单击工具栏中的 ▶ 运行 按钮，运行结果如图 9.11 所示。