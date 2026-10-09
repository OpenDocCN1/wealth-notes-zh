以下代码采用图形模式输出模型结构。

```
plot\_model(model, to\_file='tmp/model01.png')

plot\_model(model, to\_file='tmp/model02.png',show\_shapes=True, show\_layer\_names=True)
```

如图 5-3 所示是简化版模型结构图，如图 5-4 所示是增强版模型结构图。与简化版模型结构相比，增强版模型结构增加了输入输出的参数 shape 维度信息。

![](bbox=[0.15, 0.29, 0.45, 0.43])

![](bbox=[0.49, 0.29, 0.81, 0.43])

以下代码用于编译神经网络模型，以便下一步进行训练和预测分析。

```
model.compile(loss='mse', optimizer='sgd', metrics=['accuracy'])
```

第 5 组代码如下：

```
#5 模型训练
print('\n#5 模型训练 ')
model.fit(x\_train, y\_train, epochs=200, batch\_size=128)
```

调用 Keras 的 fit 训练函数，输入训练数据，对神经网络模型进行训练。

Keras 内置了 fit 训练函数，其 API 接口定义如下：

```
fit(self, x, y, batch\_size=32, epochs=10, verbose=1, callbacks=None, validation\_split=0.0, validation\_data=None, shuffle=True, class\_weight=None, sample\_weight=None, initial\_epoch=0)

fit 训练函数的常用参数如下。
```

+   - verbose: 训练时显示实时信息，0 表示不显示数据，1 表示显示进度条，2 表示只显示一个数据。
- validation\_split: 0.2 表示 20% 作为数据的验证集。
- validation\_data: 形式为 (x,y) 的元组，是指定的验证集。此参数将覆盖 validation\_split。