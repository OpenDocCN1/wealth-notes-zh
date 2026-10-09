```
In [51]: w -= u
```

```
In [52]: w
Out[52]: array([[0.41448695, [0.95214632], [0.53060197], [0.68887949]])
```

+   1. 具有二元特征的输入层。
2. sigmoid 被激活的输出层。
3. 二元标签数据。
4. 通过一阶导数反向传播。

和以前一样，需要对学习步骤进行大量迭代才能获得准确的分类结果。根据抽取的随机数，是可能达到 100% 准确率的，如下所示。

```
In [53]: steps = 3001

In [54]: a = 0.025

In [55]: w = np.random.random((features, 1))
w
Out[55]: array([[0.41253884, [0.83417131], [0.62402999], [0.66063573]])

In [56]: for s in range(1, steps + 1):
     l2 = sigmoid(np.dot(l0, w))
     e = l2 - y
     d = e * sigmoid(l2, True)
     u = a * np.dot(l0.T, d)
     w -= u
     mse = (e ** 2).mean()
     if s % 200 == 0:
         print(f'step={s:4d} | mse={mse:.4f}')
step= 200 | mse=0.1899
step= 400 | mse=0.1572
step= 600 | mse=0.1349
step= 800 | mse=0.1173
step=1000 | mse=0.1029
step=1200 | mse=0.0908
step=1400 | mse=0.0806
step=1600 | mse=0.0720
step=1800 | mse=0.0646
step=2000 | mse=0.0583
step=2200 | mse=0.0529
step=2400 | mse=0.0482
step=2600 | mse=0.0441
step=2800 | mse=0.0405
step=3000 | mse=0.0373
```