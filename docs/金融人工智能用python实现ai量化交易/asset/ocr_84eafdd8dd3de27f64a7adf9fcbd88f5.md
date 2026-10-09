```
print(f'{l:3d} | MSE={mse}')
10 | MSE=248628.10681642237
30 | MSE=731.9382249304651
50 | MSE=12.236988505004465
70 | MSE=0.7410598619743391
90 | MSE=0.0057430617304097275
110 | MSE=0.006492800939555582
```

+   1. 基于数据集的回归步骤。
2. 基于完整数据集的预测步骤。
3. 产生的 MSE 值。

神经网络在样本数据上的应用同样非常简单，类似于第 1 章中的情况。图 5-4 显示了神经网络是如何逼近样本数据的。

```
In [17]: import tensorflow as tf
tf.random.set_seed(100)
```

```
In [18]: from keras.layers import Dense
from keras.models import Sequential
Using TensorFlow backend.
```

```
In [19]: model = Sequential()
model.add(Dense(256, activation='relu', input_dim=1)) ❶
model.add(Dense(1, activation='linear')) ❷
model.compile(loss='mse', optimizer='rmsprop')
```

```
In [20]: model.summary()
Model: "sequential_1"
```

| Layer (type) | Output Shape | Param # |
| :--- | :--- | :--- |
| dense_1 (Dense) | (None, 256) | 512 |
| dense_2 (Dense) | (None, 1) | 257 |
| Total params: 769 | | |
| Trainable params: 769 | | |
| Non-trainable params: 0 | | |

```
In [21]: %time model.fit(f, l, epochs=1500, verbose=False) ❸
CPU times: user 5.89 s, sys: 761 ms, total: 6.66 s
Wall time: 4.43 s
```

```
Out[21]: <keras.callbacks.History at 0x7fc05d599d90>
```

```
In [22]: p = model.predict(f, flatten()) ❹
```

```
In [23]: MSE(l, p) ❺
Out[23]: 0.0020217512014360102
```