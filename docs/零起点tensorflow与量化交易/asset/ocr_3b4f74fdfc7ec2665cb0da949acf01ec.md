Total params: 218,859
Trainable params: 218,859
Non-trainable params: 0

输出信息中的 num_out 数值是 3，这是因为结果数据分为 3 类，采用 One-Hot 格式，占据 3 个数据字段。

如图 8-12 所示是调用 plot_model 函数生成的 LSTM 模型结构图。

![](bbox=[0.4, 0.28, 0.62, 0.65])

第 4 组代码如下：

```
#4 模型训练
print('\n#4 模型训练')
tbCallBack = keras.callbacks.TensorBoard(log_dir=rlog, write_graph=True, write_images=True)
tn0=arrow.now()
mx.fit(x_train, y_train, epochs=500, batch_size=512, callbacks=[tbCallBack])
tn=zt.timNSec('', tn0, True)
mx.save('tmp/lstm020.dat')
```