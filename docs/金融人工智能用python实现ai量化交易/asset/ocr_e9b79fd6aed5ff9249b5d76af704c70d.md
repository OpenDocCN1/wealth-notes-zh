最后，使用 Keras 创建 DNN 模型并在完整数据集上训练模型。样本内预测的基线性能约为 60%。

```
```
In [21]: import random
         import tensorflow as tf
         from keras.layers import Dense
         from keras.models import Sequential
         from keras.optimizers import Adam
         from sklearn.metrics import accuracy_score
         Using TensorFlow backend.
```
```

```
```
In [22]: def set_seeds(seed=100):
         random.seed(seed) ①
         np.random.seed(seed) ②
         tf.random.set_seed(seed) ③
```
```

```
```
In [23]: optimizer = Adam(lr=0.001) ④
```
```

```
```
In [24]: def create_model(hl=1, hu=128, optimizer=optimizer):
         model = Sequential()
         model.add(Dense(hu, input_dim=len(cols),
                         activation='relu')) ⑤
         for _ in range(hl):
             model.add(Dense(hu, activation='relu')) ⑥
         model.add(Dense(1, activation='sigmoid')) ⑦
         model.compile(loss='binary_crossentropy', ⑧
                       optimizer=optimizer,
                       metrics=['accuracy']) ⑨
         return model
```
```

```
```
In [25]: set_seeds()
        model = create_model(hl=1, hu=128)
```
```

```
```
In [26]: %%time
        model.fit(data[cols], data['d'], epochs=50,
                  verbose=False, class_weight=cw(data))
CPU times: user 6.44 s, sys: 939 ms, total: 7.38 s
Wall time: 4.07 s
```
```

```
```
Out[26]: <keras.callbacks.callbacks.History at 0x7fbf2ee6690>
```
```

```
```
In [27]: model.evaluate(data[cols], data['d'])
2103/2103 [=================================] - 0s 24us/step
```
```

```
```
Out[27]: [0.582192026280068, 0.6087952256202698]
```
```

```
```
In [28]: data['p'] = np.where(model.predict(data[cols]) > 0.5, 1, 0)
```
```

```
```
In [29]: data['p'].value_counts()
Out[29]: 1
              1340
        0
              843
        Name: p, dtype: int64
```
```