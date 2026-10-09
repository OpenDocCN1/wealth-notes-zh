+   • test_size 是样本占比，如果是整数的话，就是样本的数量。
• random_state 是随机数的种子。

但是在本案例中，并没有使用 train_test_split 数据分割函数，而是直接使用 Pandas 的函数对原数据变量 df 进行拆分的。

第 3 组代码对应的输出信息如下：

```
#3, xed.train.数据
dnum, 200 120
type, <class 'numpy.ndarray'> <class 'numpy.ndarray'>
shape, (120,) (120,)
```

以上输出信息显示，原数据共有 200 组，其中 120 组作为训练数据 df_train，剩下的 80 组数据作为测试数据 df_test。

最终应用于神经网络模型的数据经过转换，依然是 NumPy 的 ndarry 多维数组格式的，不会影响神经网络模型的训练速度。

数据格式实际转换代码如下：

```
x_train, y_train = df_train['x'].values, df_train['y'].values
x_test, y_test = df_test['x'].values, df_test['y'].values
```

注意：在转换时，请在 pandas 的字段后面加上 values，表示是数据值，否则会出现出错，很多初学者都容易在这个地方出现问题。

第 4 组代码如下：

```
#4
print('\n#4, model 建立神经网络模型')
model = zks.mlp01()
#
model.summary()
plot_model(model, to_file='tmp/model01.png')
plot_model(model, to_file='tmp/model02.png', show_shapes=True, show_layer_names=True)
#
model.compile(loss='mse', optimizer='sgd', metrics=['accuracy'])
```

建立神经网络模型，并输出相关的模型结构。
其中，以下代码通过调用 zai_keras 模块中的 mlp01 函数，快速构建单层神经网