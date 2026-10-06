
# 从零开始学 Python 大数据与量化交易

# 图 8.28 显示两只股票的开盘价信息

# 图 8.29 显示两只股票 2018 年 9 月 6 日的数据信息

# 图 8.30 显示平安银行（000001）的数据信息

## 第 9 章

## Python 大数据可视化的 Matplotlib 包

Matplotlib 包是 Python 的绘图库，它可与 Numpy 包一起使用，提供了一种有效的 MatLab 开源替代方案。Matplotlib 包最初是由 John D. Hunter 编写的，自 2012 年以来，Michael Droettboom 是主要开发者。本章首先讲解 Matplotlib 包的优点；然后讲解 figure()函数、plot()函数、subplot()函数、legend()函数及 add_axes 方法的应用；接着讲解如何设置字体格式、线条的宽度和颜色、坐标轴网格；最后讲解柱状图、色图、等高线图、立体三维图形的绘制。

### 9.1 Matplotlib 包的优点

Matplotlib 是绘制 2D 和 3D 科学图像的软件库，其优点如下。

- （1）容易学习和掌握。
- （2）兼容 LaTeX 格式的标题和文档。
- （3）可以控制图像中的每个元素，包括图像大小和扫描精度。
- （4）对于很多格式都可以高质量地输出图像，包括 PNG、PDF、SVG、EPS 和 PGF。
- （5）可以生成图形用户界面（GUI），做到交互式地获取图像以及无脑生成图像文件（通常用于批量作业）。

### 9.2 figure()函数的应用

figure()函数可以创建图形实例，其语法格式如下：

```
figure(num=None, figsize=None, dpi=None, facecolor=None, edgecolor=None, frameon=True)
```

#### 9.2.1 figure()函数的各参数意义

- num 参数：指定绘图对象的编号或名称，数字为编号，字符串为名称。
- figsize 参数：指定绘图对象的宽和高，单位为英寸。
- dpi 参数：指定绘图对象的分辨率，即每英寸多少个像素，默认值为 80。
- facecolor 参数：指定绘图对象的背景颜色。
- edgecolor 参数：指定绘图对象的边框颜色。
- frameon 参数：指定绘图对象是否显示边框。

#### 9.2.2 figure()函数的示例

要使用 figure()函数，首先要导入 matplotlib 中的 pyplot 模块。下面通过具体实例进行讲解。

默认的绘图对象示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。单击“新建”按钮，弹出下拉菜单，然后单击“文件夹”命令，就会新建一个文件夹，然后命名为 matplotlib，如图 9.1 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_203_0.png)

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

![](img/261a0e647c121c3aa8af17bcacd0d7ae_203_1.png)

# 图 9.2 默认的绘图对象示例的运行结果

含有参数的绘图对象示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“含有参数的绘图对象”，然后输入以下代码：

```
import numpy as np
from matplotlib import pyplot as plt
x = np.arange(1,25)
y = 6*x + 11
plt.figure(figsize=(12,3),dpi=120,facecolor='red')
plt.plot(x,y)
plt.show()
```

在这里，利用 figure()函数设置绘制对象的长为 12 英寸、宽为 3 英寸、分辨率为 120 像素、背景颜色为红色。

单击工具栏中的 运行 按钮，运行结果如图 9.3 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_204_0.png)

### 9.3 plot()函数的应用

plot()函数用来绘制线条或标记的轴，其语法格式如下：

```
plot(*args, **kwargs)
```

参数是一个可变长度参数，允许多个 x、y 对及可选的格式字符串。

#### 9.3.1 plot()函数的各参数意义

*args 参数：用来设置绘制线条或标记的轴的变量，如 plot(x,y)。

**kwargs：用来设置绘制线条或标记的样式和颜色，如 plot(x,y,"ob")。

样式的字符与描述如表 9.1 所示。

# 表 9.1 样式的字符与描述

| 字 符 | 描 述 |
|---|---|
| '-' | 实线样式 |
| '--' | 短横线样式 |
| '-.' | 点画线样式 |
| ':' | 虚线样式 |
| '.' | 点标记 |
| ',' | 像素标记 |
| 'o' | 圆标记 |
| 'v' | 倒三角标记 |
| '^' | 正三角标记 |
| '<' | 左三角标记 |
| '>' | 右三角标记 |
| '1' | 下箭头标记 |
| '2' | 上箭头标记 |
| '3' | 左箭头标记 |
| '4' | 右箭头标记 |
| 's' | 正方形标记 |
| 'p' | 五边形标记 |
| '*' | 星形标记 |
| 'h' | 六边形标记 1 |
| 'H' | 六边形标记 2 |
| '+' | 加号标记 |
| 'x' | X 标记 |
| 'D' | 菱形标记 |
| 'd' | 窄菱形标记 |
| '|' | 竖直线标记 |
| '_' | 水平线标记 |

颜色的字符与描述如表 9.2 所示。

# 表 9.2 颜色的字符与描述

| 字 符 | 颜 色 |
|---|---|
| 'b' | 蓝色 |
| 'g' | 绿色 |
| 'r' | 红色 |
| 'c' | 青色 |
| 'm' | 品红色 |
| 'y' | 黄色 |
| 'k' | 黑色 |
| 'w' | 白色 |

#### 9.3.2 plot()函数的实例

要使用 plot()函数，首先要导入 matplotlib 中的 pyplot 模块。下面通过具体实例进行讲解。

绘制线条示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“绘制线条”，然后输入以下代码：

```
import numpy as np
from matplotlib import pyplot as plt
x = np.arange(1,25)
y = 6*x + 11
plt.figure()
plt.plot(x,y,"oy")
plt.show()
```

在这里，设置绘制线条的样式为“圆标记”，颜色为“黄色”。单击工具栏中的 运行 按钮，运行结果如图 9.4 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_206_0.png)

图 9.4 绘制线条示例的运行结果

绘制多个线条示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“绘制多个线条”，然后输入以下代码：

```
import numpy as np
from matplotlib import pyplot as plt
x = np.arange(1,25)
y = 6 * x + 11
a = np.arange(1,8)
b = 3 * a - 6
plt.figure()
plt.plot(x,y,"oy",a,b,"*r")
plt.show()
```

在这里，设置绘制两个线条，第一个线条的样式为“圆标记”，颜色为“黄色”；第二个线条的样式为“星形标记”，颜色为“红色”。

单击工具栏中的 运行 按钮，运行结果如图 9.5 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_207_0.png)

### 9.4 subplot()函数的应用

利用 subplot()函数可以在同一图中绘制不同的图形，其语法格式如下：

```
subplot(nrows,ncols,plotNum)
```

注意：subplot()可以将 figure 划分为 n 个子图，但每条 subplot 命令只会创建一个子图。

#### 9.4.1 subplot()的各参数意义

nrows 参数：subplot 的行数。

ncols 参数：subplot 的列数。

plotNum 参数：指定的区域。

subplot 将整个绘图区域等分为 nrows 行×ncols 列个子区域，然后按照从左到右、从上到下的顺序对每个子区域进行编号，左上的子区域的编号为 1。

如果 nrows、ncols 和 plotNum 这 3 个数都小于 10，则可以把它们缩写为一个整数，如 subplot(323)和 subplot(3,2,3)是相同的。

subplot 在 plotNum 指定的区域中创建一个轴对象。如果新创建的轴和之前创建的轴重叠，则之前的轴将被删除。

#### 9.4.2 subplot()的示例

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“在同一图中绘制不同的图形”，然后输入以下代码：

```
import numpy as np
import matplotlib.pyplot as plt
# 计算正弦和余弦曲线上的点 x 和 y 的坐标
x = np.arange(0, 3 * np.pi, 0.1)
y_sin = np.sin(x)
y_cos = np.cos(x)
y_tan = np.tan(x)
# 建立 subplot 网格，高为 2、宽为 2
# 激活第一个 subplot
plt.subplot(2, 2, 1)
# 绘制第一个图形
plt.plot(x, y_sin,'ob')
plt.title('正弦曲线')
# 将第二个 subplot 激活，并绘制第二个图形
plt.subplot(2, 2, 2)
plt.plot(x, y_cos,'*m')
plt.title('余弦曲线')
# 将第三个 subplot 激活，并绘制第三个图形
plt.subplot(223)
plt.plot(x, y_tan,':r')
plt.title('正切曲线')
# 展示图像
plt.show()
```

在这里，建立 subplot 网格，为 2 行 2 列，然后调用 plot()函数绘制图形。另外，这里还调用 title()函数为图形添加标题。

单击工具栏中的 运行 按钮，运行结果如图 9.6 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_209_0.png)

### 9.5 add_axes 方法的应用

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
#新增区域 ax2,嵌套在 ax1 内
left, bottom, width, height = 0.2, 0.6, 0.25, 0.25
# 获得绘制的句柄
ax2 = fig.add_axes([left, bottom, width, height])
ax2.plot(x,y, 'b')
ax2.set_title('area2')
plt.show()
```

add_axes 方法有 4 个参数，分别是 left、bottom、width、height。left 为左侧间距、bottom 为底部间距、width 为宽度、height 为高度。需要注意的是，这 4 个参数的大小都在 0~1 之间。

单击工具栏中的 运行 按钮，运行结果如图 9.7 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_210_0.png)

### 9.6 legend()函数的应用

legend()函数是用来为图像添加图题的，其语法格式如下：
legend(*args, **kwargs)

参数是一个可变长度参数，其中最常用、最重要的可选参数是 loc 参数。loc 参数的字符与描述如表 9.3 所示。

# 表 9.3 loc 参数的字符与描述

| 字 符 | 描 述 |
|---|---|
| 0 | 由 matplotlib 确定最优位置 |
| 1 | 右上角 |
| 2 | 左上角 |
| 3 | 左下角 |
| 4 | 右下角 |
| 5 | 右侧 |
| 6 | 左中间 |
| 7 | 右中间 |
| 8 | 下中间 |
| 9 | 上中间 |
| 10 | 中间 |

为图像添加图题示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“为图像添加图题”，然后输入以下代码：

```
import numpy as np
import matplotlib.pyplot as plt
x = np.arange(1,25)
y = 6 * x + 11
fig, ax = plt.subplots()
ax.plot(x, x**2, label="y = x**2")
ax.plot(x, x**2.5, label="y = x**2.5")
ax.legend(loc=2); # 左上角
ax.set_xlabel('x 轴')
ax.set_ylabel('y 轴')
ax.set_title('为两个图形添加图题')
```

这里用到了 subplots() 函数，把图形实例 figure 分成两部分，分别绘制两个图形，具体布局由变量 fig 来完成。

利用 legend()函数为图形添加图题，并放在左上角；最后为图形实例 figure 添加 x 轴、y 轴及标题标签。

单击工具栏中的 运行 按钮，运行结果如图 9.8 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_211_0.png)

### 9.7 设置字体格式

在上述例子中，我们会发现 x 轴、y 轴及标题标签都比较小，下面就来讲解如何利用代码实现字体格式的设置。

设置字体格式示例如下。

成功登录聚宽 JoinQuant 量化交易平台后，在菜单栏中选择“我的策略”→“投资研究”命令，打开“投资研究”页面。双击 matplotlib 文件夹，然后单击“新建”按钮，在弹出的下拉菜单中单击 Python3，新建一个 Python3 文件，并命名为“设置字体格式”，然后输入以下代码：

```
import numpy as np
import matplotlib.pyplot as plt
# 更新 matplotlib 的布局参数
matplotlib.rcParams.update({'font.size': 18, 'font.family': 'serif'})
x = np.arange(1,25)
y = 6*x + 11
fig, ax = plt.subplots()
ax.plot(x, x**2, label=r'$y = \alpha^2$')
ax.plot(x, x**3, label=r'$y = \alpha^3$')
ax.legend(loc=2) # 左上角
ax.set_xlabel(r'$\alpha$')
ax.set_ylabel(r'$y$')
ax.set_title('设置字体格式')
```

在这里，利用 matplotlib.rcParams.update()更新了字体的大小及字体类型。

在 LaTeX 中经常用反斜杠符号，如用\alpha 来产生符号 α。但是反斜杠在 Python 中已经有别的含义（转义码字符）。为了避免 Python 和 LaTeX 代码混淆，采用“原始”字符串。原始字符串带有前缀“r”，如 r"\alpha"或者 r'\alpha' 而不是 "\alpha" 或'\alpha'。

单击工具栏中的 运行 按钮，运行结果如图 9.9 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_212_0.png)

图 9.9 设置字体格式示例的运行结果
