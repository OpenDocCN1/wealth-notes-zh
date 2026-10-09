```
In [20]: %timeit
model.fit(np.atleast_3d(train[cols]), train['d'],
epochs=60, batch_size=48, verbose=False,
validation_split=0.15, shuffle=False)
CPU times: user 10.1 s, sys: 1.87 s, total: 12 s
Wall time: 4.78 s
```

Out[20]: <keras.callbacks.callbacks.History at 0x7ffef3f32b18>

图 C-1 展示了训练数据集和验证数据集在不同训练轮数的性能指标。

```
In [21]: res = pd.DataFrame(model.history.history)
```

```
In [22]: res.tail(3)
Out[22]:   val_loss  val_accuracy  loss  accuracy
57   0.699932    0.508361   0.635633   0.597165
58   0.719671    0.501672   0.634539   0.598937
59   0.729954    0.505017   0.634403   0.601890
```

```
In [23]: res.plot(figsize=(10, 6));
```

![](bbox=[0.15, 0.595, 0.88, 0.89])