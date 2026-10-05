```
target = []
for i in range(200):
    if dataFile.iat[i,10] >= 7:
        target.append(1.0)
    else:
        target.append(0.0)

dataRow = dataFile.iloc[0:200,10]
plot.scatter(dataRow, target)
plot.xlabel("Attribute")
plot.ylabel("Target")
plot.show()
```

【程序 3-7】中对数据进行处理，提取了 200 行数据中的第 10 个属性，并对其进行判定，单纯的判定规则是根据均值对其区分，之后计算判定结果，如图 3-6 所示。

![](bbox=[0.23, 0.38, 0.7, 0.65])

图 3-6 大数据集中不同行相同属性之间的关系

通过图 3-6 可以看到，属性被人为地分成两部分，数据集合的程度也显示了偏离程度。如果下一步需要对属性的离散情况进行反映，那么应该使用【程序 3-8】。

# 【程序 3-8】

```
import pandas as pd
import matplotlib.pyplot as plot
    filePath = ("c://dataTest.csv")
dataFile = pd.read_csv(filePath,header=None, prefix="V")

target = []
for i in range(200):
```