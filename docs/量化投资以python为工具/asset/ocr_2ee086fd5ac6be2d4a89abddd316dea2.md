一条线用来表示变量的中位数。在矩形的上下两边各延伸出一条线，每条线的长度一般为1.5 倍的四分位距（四分位距为上下四分位数之差），这两条线被视为异常值截断线，上端的线为上限线，下端的线为下限线。在线的外面可能还会有一些点，这些点一般会被认为是异常值。箱线图能够很直观地表现出一个变量的分布，也有助于检测异常值。pyplot中的 boxplot 函数用于绘制箱线图。该函数主要参数形式为：

```
```
matplotlib.pyplot.boxplot(x, notch=None,labels=None)
```
```

+   * 参数 **x**：表示要绘制的图形数据可以是数组形式，也可以是多个向量序列；

+   * 参数 **notch**：表示箱线图的类型为布尔类型，默认取值为 **False**，表示绘制矩形箱（rect-angular box）；如果取值为 **True**，则表示绘制锯齿状箱形图（notched box）；

+   * 参数 **labels**：表示箱形图的标签，一般为字符串序列类型。

运用 boxplot( ) 函数对中国银行的开盘价、最高价、最低价和收盘价这四个变量绘制箱线图，如图 12.26 所示。

```
```python
In [12]: import numpy as np
     ...: prcData=ChinaBank.iloc[:,4]
     ...: data=np.array(prcData)
     ...: plt.boxplot(data,labels=('Open','High','Low','Close'))
     ...: plt.title('中国银行股箱线图')
Out[12]: <matplotlib.text.Text at 0x12917d7dbe0>
```
```

![](bbox=[0.07, 0.5, 0.86, 0.8])

**图 12.26 中国银行股价箱线图**

# **12.4 Figure、Axes 对象与多图绘制**

前面绘制图形时都是通过调用相关函数来设定图形的特征，比如运用 title( ) 函数来增加图形的标题，用 xlabel( ) 函数来设定 X 轴的标签。除了这种函数形式绘图以外，Mat-