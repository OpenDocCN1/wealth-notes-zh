# 第 12 章 常用第三方库：Matplotlib 库与数据可视化

# 12.2 修改图像属性

rcParams 是 Matplotlib 库中 pyplot 包绘图的参数字典，key 为'axes.unicode_minus'的默认取值（value）为 True，表示默认采用 unicode 的 minus 类型，有些字体对其的兼容支持不够，导致负号无法正常显示，现在将'axes.unicode_minus'的取值设为 False，则可以正常显示负号。

再分析如图 12.5 所示的曲线，plot() 函数默认以 $ -1 $ 和 1 为坐标的最大值和最小值。这样，我们就难以看出 1 和 $ -1 $ 的线段。此时，需要调大 Y 轴坐标的范围。X、Y 坐标轴的上下界的设定通过 xlim() 和 ylim() 函数实现，xlim() 函数用于调节 X 轴的范围，ylim() 函数用于调节 Y 轴范围。

```
matplotlib.pyplot.xlim(最小值，最大值)
matplotlib.pyplot.ylim(最小值，最大值)
```

![](bbox=[0.03, 0.34, 0.88, 0.65])

接下来，重新绘制买卖点曲线图。如图 12.6 所示。

```
#默认情况下，Matplotlib 会将连续的命令绘制在一张图上。
#ylim的第一个参数和第二个参数分别为y轴坐标的最小值和最大值
In [2]: plt.plot([1,1,0,0,-1,0,1,1,-1])
...: plt.ylim(-1.5, 1.5)
Out[2]: (-1.5, 1.5)
```

# 12.2.1.2 设定坐标标签与显示角度

除了调节坐标范围，还可以通过 xticks() 与 yticks() 函数设定坐标的标签。xticks() 与 yticks() 函数的基本用法如下：

```
matplotlib.pyplot.xticks(location, lables)
matplotlib.pyplot.yticks(location, lables)
```