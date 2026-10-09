| Layer Name | Shape | Params |
|:---|:---|:---|
| dense_3 (Dense) | (None, 256) | 512 |
| dense_4 (Dense) | (None, 256) | 65792 |
| dense_5 (Dense) | (None, 256) | 65792 |
| dense_6 (Dense) | (None, 1) | 257 |
|================================================================
| Total params: 132,353 |
| Trainable params: 132,353 |
| Non-trainable params: 0 |

```
In [34]: %time model.fit(f, l, epochs=2500, verbose=False)
    CPU times: user 34.9 s, sys: 5.91 s, total: 40.8 s
    Wall time: 15.5 s

    Out[34]: <keras.callbacks.callbacks.History at 0x7fc03fc18890>

    In [35]: p = model.predict(f).flatten()

    In [36]: MSE(L, p)
    Out[36]: 0.00046612284916401614

    In [37]: plt.figure(figsize=(10, 6))
        plt.plot(f, l, 'ro', label='sample data')
        plt.plot(f, p, '--', label='DNN approximation')
        plt.legend();
```

+   ❶ 为神经网络添加潜在的多个隐藏层。
❷ 具有 3 个隐藏层的 DNN。
❸ 这个总结展示了增加的可训练参数数量（增加的容量）。

![](bbox=[0.15, 0.58, 0.87, 0.91])