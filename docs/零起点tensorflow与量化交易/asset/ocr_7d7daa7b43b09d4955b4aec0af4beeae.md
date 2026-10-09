读取神经网络模型数据。本案例中加载的 LSTM 模型，是在案例 8-1 中生成的模型，它已经预先保存在 data/bt_lstm010mx2k.dat 文件中。

在 TopQuant 极宽智能量化系统中，支持在回溯测试中使用多种神经网络模型，模型文件采用字典模式，可以保存不同的模型数据。

```
qx.aiModel['lstm010']=mx
```

本案例只使用了 LSTM 模型。

读取模型数据后，生成模型结构图，如图 8-26 所示。

![](bbox=[0.4, 0.34, 0.6, 0.48])

以下是采用文本模式生成的模型结构。

```
#6,load_model

Layer (type)	Output Shape	Param #
--------------------------------------------
lstm_1 (LSTM)	(None, 176)	125312
--------------------------------------------
dense_1 (Dense)	(None, 1)	177
--------------------------------------------
Total params: 125,489
Trainable params: 125,489
```

第 6.2 组代码如下：

```
#6.2 var&model.wr
print('\n#6.2 var&model.wr')
fmx0='tmp/ TM_'; ztq.ai_varWr(qx,fmx0)
qx=ztq.ai_varRd(fmx0);
```