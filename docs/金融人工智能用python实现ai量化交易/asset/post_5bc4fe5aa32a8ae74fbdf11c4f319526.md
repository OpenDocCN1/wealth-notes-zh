
![](img/75a6cddadc3157a8dd9222cb5ba456ca_39_0.png)

图1-8: 随机样本数据和神经网络估计

## 1.2.3 神经网络分类

神经网络也可以很容易地用于分类任务。考虑以下基于Keras 实现神经网络分类的Python代码。二元特征数据和二元标签数据是随机生成的。建模方面的主要调整是将输出层的激活函数从linear 更改为sigmoid, 后面的章节会对此作更详细的介绍。虽然分类效果并不完美, 但是也达到了很高的准确率。图1-9 显示了准确率（表示为正确结果与所有标签值之间的关系）如何随着训练轮数而变化。准确率一开始很低，然后逐步提高，尽管不一定每一步都会有所提高。

```python
In [45]: f = 5
    n = 10

In [46]: np.random.seed(100)

In [47]: x = np.random.randint(0, 2, (n, f))
Out[47]: array([[0, 0, 1, 1, 1],
          [1, 0, 0, 0, 0],
          [0, 1, 0, 0, 0],
          [0, 1, 0, 0, 1],
          [0, 1, 0, 0, 0],
          [1, 1, 1, 0, 0],
          [1, 0, 0, 1, 1],
          [1, 1, 1, 0, 0],
          [1, 1, 1, 1, 1],
          [1, 1, 0, 0, 1]])

In [48]: y = np.random.randint(0, 2, n)
Out[48]: array([1, 1, 0, 0, 1, 1, 0, 1, 0, 1])
```

```python
In [49]: model = Sequential()
    model.add(Dense(256, activation='relu', input_dim=f))
    model.add(Dense(1, activation='sigmoid')) ①
    model.compile(loss='binary_crossentropy', optimizer='rmsprop',
                  metrics=['acc']) ②
In [50]: model.fit(x, y, epochs=50, verbose=False)
Out[50]: <keras.callbacks.callbacks.History at 0x7fde09dd1cd0>
In [51]: y_ = np.where(model.predict(x).flatten() > 0.5, 1, 0)
        y_
Out[51]: array([1, 1, 0, 0, 0, 1, 0, 1, 0, 1], dtype=int32)
In [52]: y == y_ ③
Out[52]: array([ True,  True,  True,  True,  True, False,  True,  True,  True,  True])
In [53]: res = pd.DataFrame(model.history.history) ④
In [54]: res.plot(figsize=(10, 6)); ⑤
```

- ① 创建随机特征数据。
- ② 创建随机标签数据。
- ③ 定义输出层的激活函数为sigmoid。
- ④ 定义损失函数为binary_crossentropy。
- ⑤ 将预测值与标签数据进行比较。
- ⑥ 绘制每轮训练的损失函数和准确率值。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_40_0.png)

> 注 6：损失函数用来计算和衡量神经网络（或其他机器学习算法）的预测错误。二元交叉熵（binary crossentropy）适用于二分类问题的损失函数，而 MSE 则适用于估计问题。

本节中的示例说明了与 OLS 回归相比，神经网络的一些基本特征。

### 问题无相关性

在给定一组特征值的情况下，神经网络方法的性能与需要估计或者分类的具体标签值是无关的。而统计方法（比如 OLS 回归）可能对较小的一组问题表现良好，对其他问题则表现不好或根本没有效果。

### 增量学习

给定一个用来度量成功的目标，神经网络中的最佳权重是基于随机初始化及增量改进而逐步学习得到的。这些增量改进是在考虑预测值和样本标签值之间的差异后，通过神经网络反向传播权重更新来实现的。

### 通用函数逼近器

有严格的数学定理表明神经网络（即使只有一个隐藏层）几乎可以逼近任何函数。$^7$ 这些特点或许可以解释为什么本书将神经网络放在所用算法的核心位置，第2章会在这方面进行更多的讨论。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_41_0.png)

## 1.3 数据的重要性

上一节末尾的例子表明神经网络能够很好地解决分类问题。具有一个隐藏层的神经网络在给定的数据集或样本内达到了高度的准确率。然而，神经网络的预测能力如何呢？这在很大程度上取决于可用于训练神经网络的数据量和种类。另一个基于更大数据集的数值示例将说明这一点。

### 1.3.1 小数据集

现在考虑一个与之前分类示例中使用的随机样本类似的数据集，但其具有更多特征和更多样本。人工智能中使用的大多数算法是关于模式识别的。在下面的Python代码中，二值化特征的数量定义了算法可以学习的可能模式的数量。鉴于标签数据都是二值化的，算法会尝试了解在给定某种模式的情况下（比如特征为 [0, 0, 1, 1, 1, 1, 0, 0, 0, 0]），标签是0的可能性更大，还是1的可能性更大。因为所有数字都是以相等的概率随机选择的，所以除了观察到无论在什么（随机）模式下，标签0和1的可能性都相等这一事实之外，没有太多可学习的。因此，基线预测算法在大约50%的时间内应该是准确的，无论它呈现什么（随机）模式。

```python
In [55]: f = 10
n = 250

In [56]: np.random.seed(100)

In [57]: x = np.random.randint(0, 2, (n, f)) ¹
x[:4] ¹
Out[57]: array([[0, 0, 0, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0],
                 [0, 1, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1],
                 [0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0],
                 [1, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0, 0, 0]])

In [58]: y = np.random.randint(0, 2, n) ²
y[:4] ²
Out[58]: array([0, 1, 0, 0])

In [59]: 2 ** f ³
Out[59]: 1024
```

- 1. 特征数据。
- 2. 标签数据。
- 3. 模式数量。

为了简化后续某些操作和分析，原始数据被放入一个 pandas DataFrame 对象中。

```python
In [60]: fcols = ['f'+f'_{_}'] for _ in range(f)] ¹
fcols ¹
Out[60]: ['f0', 'f1', 'f2', 'f3', 'f4', 'f5', 'f6', 'f7', 'f8', 'f9']

In [61]: data = pd.DataFrame(x, columns=fcols) ²
data['L'] = y ³

In [62]: data.info() ⁴
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 250 entries, 0 to 249
Data columns (total 11 columns):
#   Column  Non-Null Count  Dtype
0   f0      250 non-null     int64
1   f1      250 non-null     int64
2   f2      250 non-null     int64
3   f3      250 non-null     int64
4   f4      250 non-null     int64
5   f5      250 non-null     int64
6   f6      250 non-null     int64
7   f7      250 non-null     int64
8   f8      250 non-null     int64
9   f9      250 non-null     int64
10  L       250 non-null     int64
dtypes: int64(11)
memory usage: 21.6 kB
```

- 1. 定义特征数据的列名。
- 2. 将特征数据放入 DataFrame 对象中。
- 3. 将标签数据放入同一个 DataFrame 对象中。
- 4. 显示数据集的元信息。

根据以下 Python 代码的执行结果，可以确定两个主要问题：首先，并非所有模式都在样本数据集中；其次，每个观察到的模式样本量太小。即使不深入挖掘，也可以很明显地知道没有分类算法可以真正以有意义的方式了解所有可能的模式。

```python
In [63]: grouped = data.groupby(list(data.columns))

In [64]: freq = grouped['L'].size().unstack(fill_value=0)

In [65]: freq['sum'] = freq[0] + freq[1]

In [66]: freq.head(10)

Out[66]: L
            0  1  sum
0      0      0      1
        f0 f1 f2 f3 f4 f5 f6 f7 f8 f9
        0  0  0  0  0  0  0  1  1  0  1  2
                   1  0  1  0  1  1  1  1
                   1  0  1  1
                   1  0  0  0  0  1  0  1  1
                                  1  0  1  1
                   1  1  1  0  1  1  1
                   1  0  0  0  0  0  1  1  1
                                  1  0  1  1
                   1  0  0  0  1  1  1  0  1  1
                          1  1  0  0  1  0  1

In [67]: freq['sum'].describe().astype(int)

Out[67]: count    227
             mean     1
             std      0
             min      1
             25%      1
             50%      1
             75%      1
             max      2
             Name: sum, dtype: int64
```

- 1. 按列对数据进行分组。
- 2. 取消标签列分组数据的折叠。
- 3. 将 0 和 1 的频率相加。
- 4. 显示给特定定模式的 0 和 1 的频率。
- 5. 提供频率总和的统计信息。

以下 Python 代码使用了来自 scikit-learn 的 MLPClassifier 模型，该模型是在完整数据集上训练的。神经网络对于给定数据集中关系的刻画能力如何呢？正如样本内准确得分所示，这种能力非常高，接近 100%，这在很大程度上是由相对较小的数据集以及相对较高的神经网络容量所导致的结果。

> 注 8：有关详细信息，请参阅 sklearn.neural_network.MLPClassifier。

```python
In [68]: from sklearn.neural_network import MLPClassifier
from sklearn.metrics import accuracy_score

In [69]: model = MLPClassifier(hidden_layer_sizes=[128, 128, 128],
max_iter=1000, random_state=100)

In [70]: model.fit(data[fcols], data["L"])
Out[70]: MLPClassifier(hidden_layer_sizes=[128, 128, 128], max_iter=1000,
random_state=100)

In [71]: accuracy_score(data["L"], model.predict(data[fcols]))
Out[71]: 0.952
```

但是训练好的神经网络的预测能力如何呢？为此，可以将给定的数据集拆分为训练数据集和测试数据集。模型仅在训练数据集上进行训练，然后就其对测试数据集的预测能力进行测试。和以前一样，训练好的神经网络在样本内（训练数据集）的准确率非常高。然而，它比测试数据集上的基线算法差 10 个百分点以上。

```python
In [72]: split = int(len(data) * 0.7) 1

In [73]: train = data[:split] 1
test = data[split:] 1

In [74]: model.fit(train[fcols], train["L"]) 2
Out[74]: MLPClassifier(hidden_layer_sizes=[128, 128, 128], max_iter=1000,
random_state=100)

In [75]: accuracy_score(train["L"], model.predict(train[fcols])) 3
Out[75]: 0.9714285714285714

In [76]: accuracy_score(test["L"], model.predict(test[fcols])) 4
Out[76]: 0.38666666666666666
```

- 1. 将数据集拆分为训练数据集和测试数据集。
- 2. 仅在训练数据集上训练模型。
- 3. 报告样本内（训练数据集）的准确率。
- 4. 报告样本外（测试数据集）的准确率。

粗略地说，由于两个已知的问题，仅在小数据集上训练的神经网络会学习到错误的关系。这些问题对于样本内的关系学习影响不大。相反，一般来说，数据集越小，越容易学习样本内关系。然而，当使用训练好的神经网络进行样本外预测时，这些问题会带来麻烦。

### 1.3.2 更大的数据集

幸运的是，通常有一种明确的方法可以解决这种情况下的问题，即创建更大的数据集（或者说更多的数据）。而对现实世界的问题，理论上确实可以这样做事。然而，从实际的角度来看，我们并不总是能够获得这种更大的数据集，而且它们通常也不那么容易生成。但是，基于本节的示例，确实可以通过代码轻松创建更大的数据集。

以下 Python 代码显著增加了初始样本数据集中的样本数量。结果表明，训练好的神经网络的预测准确率提高了 10 个百分点以上，达到约 50% 的水平，这也是我们基于标签数据的生成逻辑所预期的。它现在的性能与一个标准的基线算法相符合。

```python
In [77]: factor = 50

In [78]: big = pd.DataFrame(np.random.randint(0, 2, (factor * n, f)),
                         columns=fcols)

In [79]: big['L'] = np.random.randint(0, 2, factor * n)

In [80]: train = big[:split]
        test = big[split:]

In [81]: model.fit(train[fcols], train['L'])
Out[81]: MLPClassifier(hidden_layer_sizes=[128, 128, 128], max_iter=1000,
                     random_state=100)

In [82]: accuracy_score(train['L'], model.predict(train[fcols])) ①
Out[82]: 0.9657142857142857

In [83]: accuracy_score(test['L'], model.predict(test[fcols])) ②
Out[83]: 0.5043407787910751
```

- ① 样本内的预测准确率（训练数据集）。
- ② 样本外的预测准确率（测试数据集）。

接下来的代码所示，我们可以通过对可用数据的快速分析，来解释为什么预测准确率有所提高。首先，因为数据量足够大，所以所有可能的模式现在都会出现在数据集中。其次，所有模式在数据集中的平均频率都在 10 以上。换句话说，对**每一种可能的模式**，神经网络都可以观测和学习**多次**。这使得神经网络可以“学习”到，标签 0 和 1 对于所有模式的可能性是相等的。当然，这是一种相当复杂的学习方式，但它很好地说明了一个事实：在基于神经网络的设置下，相对较小的数据集可能是不够的。

```python
In [84]: grouped = big.groupby(list(data.columns))

In [85]: freq = grouped['L'].size().unstack(fill_value=0)

In [86]: freq['sum'] = freq[0] + freq[1] ①

In [87]: freq.head(6)
Out[87]: l     0     1   sum
              f0  f1  f2  f3  f4  f5  f6  f7  f8  f9
0      0    0    0    0    0    0    0    0    0   10    9   19
1                              1   4   4       9
2                            1   0   2   5   7
3                          1    6   6   12
4                        1   0    9   8   17
5                      1    7   4   11
```
