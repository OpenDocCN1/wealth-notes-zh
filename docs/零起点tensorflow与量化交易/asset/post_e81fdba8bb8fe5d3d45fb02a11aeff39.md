
Keras 模型可视化的第一种方案是调用内置的 `summary` 函数，生成一个文本网络模型。

以下代码采用文本模式打印出模型概况。

```python
model.summary()
```

其对应的输出信息如下：

| Layer (type) | Output Shape | Param # |
|---|---|---|
| mlp01 (Dense) | (None, 1) | 2 |

Total params: 2
Trainable params: 2
Non-trainable params: 0

Keras 模型可视化的第二种方案，是调用内置的 `plot_model` 函数。`plot_model` 函数可以生成类似于流程图的图像模型。`plot_model` 函数需要 `graphviz`、`plotdot` 等模块库的支持。

以下代码采用图形模式输出模型结构。

```python
plot_model(model, to_file='tmp/model01.png')
plot_model(model, to_file='tmp/model02.png', show_shapes=True, show_layer_names=True)
```

Keras 模型可视化的第三种方案是通过回调模式，间接支持 TensorBoard。具体参见案例程序的第 5 组代码：

```python
# 5 模型训练
print('\n#5 模型训练')
tbCallBack = keras.callbacks.TensorBoard(log_dir=r'log', write_graph=True, write_images=True)
model.fit(x_train, y_train, epochs=200, batch_size=128, callbacks=[tbCallBack])
```

Keras 的 callbacks 回调函数涉及很多底层的编程，使用非常复杂，有关细节请大家参考相关文档。

对于初学者和量化编程而言，只需获得最终的 TensorBoard 神经网络模型数据即可，其他细节可以无须考虑，因此可以采用黑箱模式，记住以上代码格式即可。

与前面两种可视化编程不同，第三种模型可视化编程不能直接生成模型结构，必须通过调用 `fit` 训练函数不断积累数据才行，这也间接说明了 TensorBoard 本质上只是一种 log 数据的可视化分析软件，不过是基于神经网络模型的 log 数据的。

调用 `fit` 训练函数进行模型训练后，对于 zwPython 平台，则调用对应版本下的 WinPython Command Prompt.exe 程序进入 DOS 命令行。初学者很容易在这个环节出现各种错误，因此需要特别注意。

关于其他的 Python 开发环境，请参考相关文档，将 Python 与系统进行绑定，或者调用类似的命令行程序。

不能直接使用 Windows 系统内置的命令行程序，因为无法绑定 Python 运行环境。在默认情况下，zwPython 会进入 `scripts` 目录下，调用我们编写的 `tbtmp.bat` 批处理程序，即可启动 TensorBoard 程序。

`tbtmp.bat` 文件名当中的 “tb” 是 TensorBoard 的缩写，“tmp” 是目录 `ailib/log_tmp` 的缩写。

其脚本内容如下：

```batch
tensorboard --logdir=ailib\log_tmp
```

表示使用 `ailib/log_tmp` 这个常用的临时 log 目录，快速调用 TensorBoard 可视化程序，调用时无需任何参数。

进入支持 Python 的命令行窗口后，直接输入：

```bash
tbtmp
```

即可激活 TensorBoard 可视化程序，如图 5-6 所示。

tbtmp.bat 是 Windows 版本的快速启动脚本，其他平台如 Mac、Linux 等请参考相关的 shell 脚本信息自行编写。

TensorBoard 是基于 Web 的可视化系统，激活后需要通过浏览器才能使用，步骤如下。

- 打开常用的浏览器，最好是谷歌内核的浏览器，IE 内核的浏览器可能有兼容问题。

![](img/78eb676c584abe249cda782e9c1dd99b_189_0.png)

图 5-6 调用 tbtmp.bat 脚本

- 在地址栏输入：`http://localhost:6006/`，按回车键或单击浏览按钮。

在进入前需要等待数秒，因为系统在读取 log 数据。如图 5-7 所示是进入 TensorBoard 后的界面。

![](img/78eb676c584abe249cda782e9c1dd99b_189_1.png)

图 5-7 进入 TensorBoard 后的界面

单击 “GRAPH”，显示 mlp1 神经网络模型的结构图，如图 5-8 所示。

## 案例 5-3：模型读写

从图 5-8 可以看出，TensorBoard 不愧为专业级的神经网络可视化分析系统，连底层的调用结构都清清楚楚。

从 mlp01 神经网络模型结构图我们也可以发现，即使是最简单的神经网络模型，只有一层神经网络，其内部结构也是非常复杂的。

有关 TensorBoard 的具体使用，大家可以阅读《零起点 TensorFlow 快速入门》，以及其他相关文档。

在机器学习、神经网络案例中，对模型进行一次完整的训练，经常需要花费一整天甚至数目的时间，因此需要经常对训练的模型和相关权重数据作保存。

在实盘分析中，往往耗时最多的是机器学习构建算法模型，在真正预测预段，数据反而不多。

因为建模使用的是历史数据，通常有数万条记录，甚至达到百万、千万级别，某些大数据项目更是以亿单位为起点。

而预测往往只采用实时数据，一般都是当天的最新数据，再配合一些类似于均线的数据等，数据量并不大。

以往案例都是把建模和预测放在一起，如果能够分开，在进行实盘分析时，无须建模，直接预测，那么速度至少可以提高 10 倍以上。

![](img/78eb676c584abe249cda782e9c1dd99b_190_0.png)

目前，常用的机器学习平台基本上都提供了模型读写工具。

- TensorFlow 提供了一个 Saver 类，用于在训练完一个模型之后，保存训练结果。该结果指的是模型参数，可以用于日后的训练或者测试。
- Python 的 pickle 模块支持二进数据或程序的一体化存储，是非常理想的机器学习模型存储工具。
- SKLearn 模块中内置了高效的模型持久化模块 joblib，将模型保存至硬盘，并且从硬盘加载效率更高。
- 在 Keras 的 model 模块中，也提供了多种神经网络模型存储和调用的语句。

案例 5-3 文件名为 `kc503_mwr.py`，主要介绍 Keras 神经网络模型保存和读取的具体方法。

与前面案例相比，案例 5-3 只是增加了部分神经网络模型保存和读取的语句，为节省篇幅，下面我们只针对相关代码进行讲解。

案例 5-3 分别保存了两种不同的模型，其中一种是调用 `fit` 函数训练、已经有各种内置参数的权重数据的模型；另一种是没有调用 `fit` 训练函数、没有权重数据的空白模型。

下面先讲解第一种模型，即空白模型的保存，参见案例程序中的第 4 组代码。

```python
# 4
print('\n#4，model建立神经网络模型')
model = zks.mlp01()
model.summary()
#
model.compile(loss='mse', optimizer='sgd', metrics=['accuracy'])
#
model.save('tmp/mlp01.dat')
```

调用 `zks.mlp01` 函数，建立 `model` 模型变量后，直接使用 `save` 函数把神经网络模型保存到文件中。

在保存神经网络模型前，调用了 `compile` 函数编译模型。实际上，不调用编译函数，也可以保存。

事实上，在优化模型参数时，不调用 `compile` 函数编译模型，可以对编译环节进行部分优化。

这里保存通过 `compile` 函数编译好的模型，是为了提高后面参数优化的运行速度，使得每一组参数无须再调用 `compile` 函数编译模型。第 5 组代码如下：

```python
# 5 模型训练
print('\n#5 模型训练')
mx = load_model('tmp/mlp01.dat')
mx.fit(x_train, y_train, epochs=200, batch_size=128)
#
mx.save('tmp/mlp02x.dat')
```

首先使用 `load_model` 函数加载预先保存的模型。注意，为了相互区别，读入后的模型使用了一个新的变量名称 `mx`。
然后运行 `mx` 的 `fit` 训练函数，训练模型。
完成训练后，使用新的文件名保存新的模型变量 `mx`，第二次保存的模型已经包含了相关权重数据。
两种模型的保存文件如图 5-9 所示。

![](img/78eb676c584abe249cda782e9c1dd99b_192_0.png)

第二次保存的模型文件 `mlp02x.dat` 的大小是 17KB，第一次保存的模型文件的大小只有 12KB，这是因为在第二次保存的模型文件中，增加了大量的内部参数的权重数据。第 6 组代码如下：

```python
# 6 整理预测数据
print('\n#6 整理预测数据 ')
_y_pred = mx.predict(x_test)
df_test['y_pred'] = xdat.ds4x(y_pred, df_test.index, True)
dacc, _ = ztg.ai_acc_xsd2x(df_test, y, df_test['y_pred'], 5, False)
print('acc:', dacc)
```

调用 `predict` 预测函数，测试模型的预测结果。其对应的输出信息如下：

```
#6 整理预测数据
acc: 88.75
```

准确度是 88.75%，和前面的案例完全一致。

如果我们采用第二种保存模式，将神经网络模型的结构、参数权重一起保存，则无须再运行 `fit` 训练函数，直接运行第 6 组代码，调用 `predict` 预测函数进行数据预测即可。在实盘分析中，这也是最常用的模式。

由本案例可以看出，我们可以预先将训练模型保存为文件，在进行实盘数据分析时再直接读取，这样可以节约大量的训练时间。

## 案例 5-4：参数调优入门

做生意，讲究有进有出。同样，我们存储模型的目的是为了使用。

案例 5-4 文件名是 `kc504_mvar.py`，主要介绍如何使用所保存的模型文件，快速进行不同的参数测试，从而找到最好的参数。

案例 5-4 程序很简单，其大部分代码前面都介绍过，真正和参数测试有关的代码，是如下第 4 组代码。

```python
#
fm_x = 'data\\mlp01.dat'
ft_q = 'tmp/df5.csv'
#
df2 = ztq.ai_mx_tst_epochs(fm_x, ft_q, df_train, df_test, nepochs=100, nsize=128, ky0=5)
# df2 = ztq.ai_mx_tst_bsize(fm_x, ft_q, df_train, df_test, nepochs=300, ksize=8, ky0=5)
# df2 = ztq.ai_mx_tst_kacc(fm_x, ft_q, df_train, df_test, nepochs=300, nsize=128)
```

真正有用的就如下一条语句：

```python
df2 = ztq.ai_mx_tst_epochs(fm_x, ft_q, df_train, df_test, nepochs=100, nsize=128, ky0=5)
```

调用 `ztools_tq` 模块中的 `ai_mx_tst_epochs` 函数，测试不同参数的准确度，并绘制相关的图形。

以下是输出信息。

```
df2
```

| epoch_acc | nepoch | ntim |
|---|---|---|
| 0 | 46.250 | 100 |
| 1 | 88.750 | 200 |
| 2 | 88.750 | 300 |
| 3 | 88.750 | 400 |
| 4 | 88.750 | 500 |
| 5 | 88.750 | 600 |
| 6 | 88.750 | 700 |
| 7 | 88.750 | 800 |
| 8 | 88.750 | 900 |
| 9 | 88.750 | 1000 |

如图 5-10 和图 5-11 所示是对应的输出图形。

![](img/78eb676c584abe249cda782e9c1dd99b_194_0.png)

图 5-10 训练轮数 epochs 参数测试

`ai_mx_tst_epochs` 函数自动按 100~1000 轮，每轮递增 100 来测试不同的训练轮数。从图 5-10 和输出信息可以看出，由于案例中的 `mlp01` 模型属于最简单的单层神经网络模型，经过 300 轮的 训练，模型就趋向稳定，准确度已经达到 88.75%。再增加训练轮数，则属于过度迭代，对于精度的提高完全没有任何帮助。

`epochs`（迭代轮数）和 `batch-size`（每批数据规模）等参数，往往与模型的运行速度有很大关系，通常测试不同的参数组合，在同样的效果下，优先挑选速度最快的参数组合。

# 第5章 单隐层神经网络预测股价

![](img/78eb676c584abe249cda782e9c1dd99b_195_0.png)

图 5-11 训练轮数 epochs 参数测试时间

从图 5-11 和输出信息可以看出，`epochs` 参数取 200 以后，`epoch_acc` 都是一样的，即都为 88.75%，不过训练时间越来越长。

本书教学课件中的案例通常都采用 500 轮迭代，不过 `mlp01` 模型简单，`epochs` 参数取 300 即可，以节省程序运行时间。

300 这个数值，就是我们通过 `ai_mx_tst_epochs` 函数找到的 `epochs` 优化参数。`ai_mx_tst_epochs` 函数的源码位于 `ztools_tq` 模块中，也很简单，就是一个 for 循环，请大家自行分析。

在第 4 组代码中，其实通过了三组不同的参数优化测试。

```python
df2 = ztq.ai_mx_tst_epochs(fm_x, ft_q, df_train, df_test, nepochs=100, nsize=128, ky0=5)
# df2 = ztq.ai_mx_tst_bsize(fm_x, ft_q, df_train, df_test, nepochs=300, ksize=8, ky0=5)
# df2 = ztq.ai_mx_tst_kacc(fm_x, ft_q, df_train, df_test, nepochs=300, nsize=128)
```

请大家轮流调用测试函数，看看不同的测试效果。

以下是调用 `ai_mx_tst_bsize` 函数测试的效果。

| bsize | ntim | size_acc |
|---|---|---|
| 8 | 17.380 | 88.750 |
| 16 | 10.390 | 88.750 |
| 24 | 8.030 | 88.750 |
| 32 | 7.070 | 88.750 |
| 40 | 5.770 | 88.750 |
| 48 | 6.080 | 88.750 |
| 56 | 6.130 | 88.750 |
| 64 | 4.960 | 88.750 |
| 72 | 5.160 | 88.750 |
| 80 | 5.350 | 88.750 |

如图 5-12 和图 5-13 所示是对应的输出图形。

![](img/78eb676c584abe249cda782e9c1dd99b_196_0.png)

![](img/78eb676c584abe249cda782e9c1dd99b_196_1.png)

在正常情况下，**batch-size** 应该是 32 的倍数。由于本案例中的 **mlp01** 模型非常简单，所以我们采用 8 的倍数进行测试。

从图 **5-12** 和输出信息可以看出，不同的 **batch-size** 对应的输出结果是一样的。

从图 5-13 和输出信息可以看出，`bat-size` 越大，模型测试速度越快。

最后运行 `ai_mx_tst_kacc` 函数，采用不同的误差精度测试模型的准确度。由于精度测试是在模型训练后完成的，与时间关联度不大，所以就省略了时间参数。

其对应的输出信息如下：

| dif | dacc | kacc |
| :--- | :--- | :--- |
| 0 | 60.000 | 1 |
| 1 | 73.750 | 2 |
| 2 | 82.500 | 3 |
| 3 | 83.750 | 4 |
| 4 | 88.750 | 5 |
| 5 | 90.000 | 6 |
| 6 | 90.000 | 7 |
| 7 | 91.250 | 8 |
| 8 | 91.250 | 9 |
| 9 | 91.250 | 10 |

如图 5-14 所示是对应的输出图形。

![](img/78eb676c584abe249cda782e9c1dd99b_197_0.png)

由于本案例比较简单，误差精度超过 8% 以后，准确度都为 91.25%。默认的误差宽容度是 5%，对于实盘分析而言，一般采用 2%~3% 的误差精度，具体数值要参考数据的波动程度。
