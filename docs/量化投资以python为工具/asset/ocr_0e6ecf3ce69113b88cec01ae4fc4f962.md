```
# a map from key -> value, converter
defaultParams = {
    
    
    
    'font.family': [['sans-serif'], validate_stringlist],# used by text object
    
    
    
    'text.sans-serif': [['SimHei', 'Bitstream Vera Sans', 'DejaVu Sans',
'Luxi Sans', 'Luxi Sans',
'Lucida Grande', 'Verdana', 'Geneva', 'Lucid',
'Arial', 'Helvetica', 'Avenir Garde', 'sans-serif'],
validate_stringlist],
    
    
    
}
```

+   * 修改负号显示: 在key取值为 “axes.unicode_minus” 的取值中，将 True 改为 False。如下所示:

```
# a map from key -> value, converter
defaultParams = {
    
    
    
    'axes.unicode_minus': [False, validate_bool],
    
    
    
}
```

保存上述修改并关闭 rcsetup.py 文件。

经过上述修改，在以后使用 Matplotlib 绘图时，图像就可以正常显示中文与符号了。

# 12.2.2.3 设定坐标轴标签

在分析图形内容时，首先要搞清楚图中横纵坐标分别代表的含义。在图像中添加坐标轴标题会直观显示坐标轴代表的数据变量。X 轴、Y 轴的标签设定分别通过 xlabel() 和 ylabel() 函数来实现，这两个函数也位于 pyplot 包。

```
xlabel('x')
ylabel('y')
```

下面，为中国银行价格曲线图设定坐标轴标签，图形结果如图 12.11 所示。

```
In [7]: plt.plot(Close['2019'])
....: plt.title('中国银行2019年收盘价')
....: plt.xlabel('日期')
....: plt.ylabel('收盘价')
Out[7]: <matplotlib.text.Text at 0x1fd89734198>
```