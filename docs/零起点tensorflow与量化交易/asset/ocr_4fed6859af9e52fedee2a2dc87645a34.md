| | | |
|---|---|---|
| lstm_1 (LSTM) | (None, 176) | 125312 |
| dense_1 (Dense) | (None, 1) | 177 |
| = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = | | |
| Total params: 125,489 | | |
| Trainable params: 125,489 | | |
| Non-trainable params: 0 | | |

如图 8-20 所示是调用 plot_model 函数生成的 LSTM 模型结构图。

![](bbox=[0.39, 0.31, 0.61, 0.49])

图 8-20 LSTM 模型结构图

第 4 组代码如下：

```
#4模型训练
print('\n#4模型训练')
tbCallBack=keras.callbacks.TensorBoard(log_dir=rlog,write_graph=True, write_images=True)
tn0=arrow.now()
mx.fit(x_train, y_train, epochs=2000, batch_size=512*24,callbacks=[tbCallBack])
tn=zt.timNSec('',tn0,True)
mx.save('tmp/lstm010mx.dat')
```

调用 fit 训练函数，输入数据，对 mx 模型变量进行训练，同时采用 CallBack 回调模式，记录相关的 TensorBoard 日志数据。

第 4 组代码最后的输出信息如下：

```
Epoch 1995/2000
154729/154729[============ ==============] - 40s - loss: 0.2323 - acc: 0.0087
Epoch 1996/2000
```