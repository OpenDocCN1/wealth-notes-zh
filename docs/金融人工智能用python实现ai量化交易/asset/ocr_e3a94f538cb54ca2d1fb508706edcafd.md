```
features = ['symbol', 'r', 'd', 'sma', 'min', 'max', 'mom', 'vol']
for f in features:
    for lag in range(1, lags + 1):
        col = f'{f}_{lag}'
        df[col] = diff.shift(lag)
        cols.append(col)
df.dropna(inplace=True)
return df, cols
```

```
In [13]: data, cols = add_lags(data, symbol, lags)
```

**1** 基于第 6 章的函数进行微调。

其次，看一下标签数据。取决于可用的数据集，分类中可能出现的一个主要问题是**类别不平衡**。这意味着在二分类标签中一个特定类别的频率可能高于另一个类别。这可能会导致神经网络简单地以更高的频率预测类别，因为这已经可以导致低损失和高准确率。应用适当的权重，可以确保两个类在 DNN 训练步骤中获得同等重要性。

```
In [14]: len(data)
Out[14]: 2183
```

```
In [15]: c = data['d'].value_counts()  # 1
Out[15]: 0    1445
        1     738
Name: d, dtype: int64
```

```
In [16]: def cw(df):  # 2
        c0, c1 = np.bincount(diff['d'])
        w0 = (1 / c0) * (len(df)) / 2
        w1 = (1 / c1) * (len(df)) / 2
        return {0: w0, 1: w1}
```

```
In [17]: class_weight = cw(data)  # 3
```

```
In [18]: class_weight  # 3
Out[18]: {0: 0.755363321799308, 1: 1.4789972899728998}
```

```
In [19]: class_weight[0] * c[0]  # 3
Out[19]: 1091.5
```

```
In [20]: class_weight[1] * c[1]  # 3
Out[20]: 1091.5
```

+   **1** 显示两个类的频率。

+   **2** 计算适当的权重以达到相等的权重。

+   **3** 根据计算出的权重，两个类的权重相等。

> 注 2：请参阅博客文章 “*Practical Guide to Handling Imbalanced Datasets*” ，其中讨论了使用 Keras 解决类别不平衡问题。