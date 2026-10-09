```
treward += reward if not done else 0  
prev_state = state  
if treward >= 110:  ①
    data = data.append(pd.DataFrame(results))  ②
                  length.append(treward)  ③
CPU times: user 9.84 s, sys: 48.7 ms, total: 9.89 s
Wall time: 9.89 s
In [10]: np.arary(length).mean()  ④
Out[10]: 119.75
In [11]: data.info()  ⑤
\<class 'pandas.core.frame.DataFrame'\>
Int64Index: 479 entries, 0 to 143
Data columns (total 6 columns):
#   Column  Non-Null Count  Dtype
0        s1        479 non-null    float64
1        s2        479 non-null    float64
2        s3        479 non-null    float64
3        s4        479 non-null    float64
4        a         479 non-null    int64
5        r         479 non-null    float64
dtypes: float64(5), int64(1)
memory usage: 26.2 KB
In [12]: data.tail()  ⑤
Out[12]:          s1          s2         s3          s4   a     r
139   0.639509   0.992699  -0.112029  -1.548863   1   1.0
140   0.659363   0.799086  -0.143006  -1.293311   0   1.0
141   0.675345   0.600842  -0.168689  -1.048421   0   1.0
142   0.687466   0.433513  -0.189937  -0.813148   1   1.0
143   0.695736   0.610658  -0.206100  -1.159030   0   1.0
```

+   1. 仅当随机智能体的总奖励至少为 100 时……
2. ………收集数据……
3. ………并记录总奖励。
4. 数据集中包含的所有随机游戏的总奖励平均值。
5. 查看 DataFrame 对象中收集的数据。

准备好数据后，可以按如下方式训练神经网絡：建立神经网络进行分类，将状态数据作为特征，将采取的操作作为标签数据。鉴于数据集合仅包含在给定状态下成功的动作，神经网络会了解在给定状态（特征）下要采取的动作（标签）。

```
In [13]: from pylab import plt
plt.style.use('seaborn')
\%matplotlib inline
In [14]: import tensorflow as tf
tf.random.set_seed(100)
```