```
In [49]: model = Sequential()
    model.add(Dense(256, activation='relu', input_dim=f))
    model.add(Dense(1, activation='sigmoid')) Ⓢ
    model.compile(loss='binary_crossentropy', optimizer='rmsprop',
                  metrics=['acc']) ⓐ
In [50]: model.fit(x, y, epochs=50, verbose=False)
Out[50]: <keras.callbacks.callbacks.History at 0x7fde09dd1cd0>
In [51]: y_= np.where(model.predict(x).flatten() > 0.5, 1, 0)
        y_
Out[51]: array([1, 1, 0, 0, 0, 1, 0, 1, 0, 1], dtype=int32)
In [52]: y == y_ ⓐ
Out[52]: array([ True,  True,  True,  True,  True, False,  True,  True,  True,  True,
        True])
In [53]: res = pd.DataFrame(model.history.history) ⓐ
In [54]: res.plot(figsize=(10, 6)); ⓐ
```

+   ① 创建随机特征数据。
② 创建随机标签数据。
③ 定义输出层的激活函数为sigmoid。
④ 定义损失函数为binary_crossentropy。
⑤ 将预测值与标签数据进行比较。
⑥ 绘制每轮训练的损失函数和准确率值。

![](bbox=[0.13, 0.56, 0.85, 0.84])

> 注 6：损失函数用来计算和衡量神经网络（或其他机器学习算法）的预测错误。二元交叉熵（binary crossentropy）适用于二分类问题的损失函数，而 MSE 则适用于估计问题。