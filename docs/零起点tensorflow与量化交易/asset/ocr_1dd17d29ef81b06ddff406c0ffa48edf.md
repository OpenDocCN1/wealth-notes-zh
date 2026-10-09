代码很简单，核心部分只有一行代码，通过股票池主调用函数 tq_pools_call，根据股票代码调用数据预处理函数 preFun，对相关数据进行处理。

通过 tq_pools_call 函数，我们可以充分感受到 Python 语言的优雅与强大，简简单单几行代码，就可以完成其他语言的若干模块难以完成的工作。

第 6 组代码如下：

```
#6 load model
print('\n#6,load model')
mx=load_model('data/bt_lstm010mx2k.dat') ###
qx.aiModel['lstm010']=mx
#
mx.summary()
plot_model(mx, to_file='tmp/lstm010bt.png')
```

读取神经网络模型数据本案例中加载的 LSTM 模型，是在案例 8-1 中生成的模型，它已经预先保存在 data/bt_lstm010mx2k.dat 文件中。

读取模型数据后，生成模型结构图，如图 8-44 所示。

![](bbox=[0.35, 0.52, 0.7, 0.75])

图 8-44 模型结构图

以下是采用文本模式生成的模型结构。

```
#6,load_model

Layer (type) Output Shape Param #
=========================================================================
lstm_1 (LSTM) (None, 176) 125312
```