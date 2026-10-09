```
```python
model.add(Layer(hu))
model.add(Dense(1, activation='sigmoid'))
model.compile(optimizer='noptimizor',
loss='binary_crossentropy',
metricss=['accuracy'])
return model

In [106]: set_seeds(1)
model = create_deep_rnn_model(
h1=2, hu=50, layers='SimpleRNN',
features=len(data.columns),
dropout=True, rate=0.3)

In [107]: %time
model.fit_generator(g, epochs=200, steps_per_epoch=10,
verbose=False, class_weight=cl(train_y))
CPU times: user 14.2 s, sys: 2.85 s, total: 17.1 s
Wall time: 7.09 s

Out[107]: <keras.callbacks.callbacks.History at 0x7fe7ef6428790>

In [108]: y = np.where(model.predict(g, batch_size=None) > 0.5, 1, 0).flatten()

In [109]: np.bincount(y)
Out[109]: array([550, 324])

In [110]: accuracy_score(test_ylags:1), y)
Out[110]: 0.6430205949655751
```

```

+   ① 保证最少有两个隐藏层。
② 第一个隐藏层。
③ Dropout 层。
④ 最终隐藏层。
⑤ 建立分类模型。

# ## 8.6 结论

本章基于 Keras 库介绍了 RNN 模型，并说明了这种神经网络在金融时间序列数据中的应用。从 Python 语言的角度看，使用 RNN 与使用 DNN 并无太大区别。一个主要区别是训练数据和测试数据必须以序列形式呈现给各自的方法，但是通过应用 TlmeseriesGenerator 函数可以轻松实现这一点，该函数使用的生成器对象会将序列数据转换为 Keras 中的 RNN 能处理的数据。

本章中的示例适用于金融价格序列和金融收益率序列。此外，我们还可以轻松添加金融特征，比如时间序列动量。为模型创建提供的函数可以使用 SimpleRNN 层或 LSTM 层以及不同的优化器等，还可以在浅层神经网络和深层神经网络的背景下对估计和分类问题进行建模。

在预测市场方向时，分类实例得到的样本外预测准确率相对较高，但对估计实例而言并没有那么高，甚至可能很低。