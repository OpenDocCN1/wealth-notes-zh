```
dataRow1 = dataFile.iloc[100,1:300]
    dataRow2 = dataFile.iloc[101,1:300]
    plot.scatter(dataRow1, dataRow2)
    plot.xlabel("Attribute1")
    plot.ylabel("Attribute2")
    plot.show()
```

从【程序 3-6】可以看到，首先使用 filePath 创建了一个文件路径，用以建立数据地址。之后使用 pandas 自带的 read_csv 读取 CSV 格式的文件。dataFile 是读取的数据集，之后使用 iloc 方法获取其中行的属性数据，scatter 是做出分散图的方法，对属性进行画图。最终结果如图 3-5 所示。

![](bbox=[0.26, 0.3, 0.76, 0.59])

图 3-5 大数据集中不同目标属性之间的关系

可以看到，数据在 (0,0) 的位置有较大的集合，表明属性在此的偏离程度较小，而几个特定点是偏离程度较大的点。这可以帮助读者对离群值进行分析。

提示
在【程序 3-6】出现了两幅图，请读者自行分析。

下面继续对数据集进行分析。【程序 3-5】和【程序 3-6】让读者看到了对数据的同一行中不同的属性进行处理的方法，如果要对不同目标行的同一种属性进行分析，那么如何做呢？请读者参阅【程序 3-7】。

# 【程序 3-7】

```
import pandas as pd
import matplotlib.pyplot as plot
    filePath = ("c://dataTest.csv")
dataFile = pd.read_csv(filePath,header=None, prefix="V")
```