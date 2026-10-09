
# 目录

## 第 4 章
- 4.6.1 线性回归模型……128
- 案例 4-4：上证指数 n+1 的开价预测……129
- 案例 4-5：预测数据评估……133
- 4.6.2 效果评估函数……136
- 4.6.3 常用的评测指标……138
- 4.7 n+1 大盘指数趋势预测……139
- 案例 4-6：涨跌趋势归一化分类……140
- 案例 4-7：经典版涨跌趋势归一化分类……143
- 4.8 One-Hot……145
- 案例 4-8：One-Hot 格式……146
- 4.9 DNN 模型……149
- 案例 4-9：DNN 趋势预测……150

## 第 5 章 单层神经网络预测股价
- 5.1 Keras 简化接口……156
- 5.2 单层神经网络……158
- 案例 5-1：单层神经网络模型……158
- 5.3 神经网络常用模块……168
- 案例 5-2：可视化神经网络模型……170
- 案例 5-3：模型读写……174
- 案例 5-4：参数调优入门……177

## 第 6 章 MLP 与股价预测
- 6.1 MLP……182
- 案例 6-1：MLP 价格预测模型……183
- 6.2 神经网络模型应用四大环节……189
- 案例 6-2：MLP 模型评估……190
- 案例 6-3：优化 MLP 价格预测模型……194
- 案例 6-4：优化版 MLP 模型评估……197

## 第 7 章 RNN 与趋势预测
- 7.1 RNN……200
- 7.2 RNN 与趋势预测……201
- 案例 7-1：RNN 趋势预测模型……201
- 案例 7-2：RNN 模型评估……209
- 案例 7-3：RNN 趋势预测模型 2……211
- 案例 7-4：RNN 模型 2 评估……214

## 第 8 章 LSTM 与量化分析
- 8.1 LSTM 模型……217
  - 8.1.1 数值预测……218
  - 案例 8-1：LSTM 价格预测模型……219
  - 案例 8-2：LSTM 价格预测模型评估……226
  - 8.1.2 趋势预测……230
  - 案例 8-3：LSTM 股价趋势预测模型……231
  - 案例 8-4：LSTM 趋势模型评估……239
- 8.2 LSTM 量化回测分析……242
  - 8.2.1 构建模型……243
  - 案例 8-5：构建模型……243
  - 8.2.2 数据整理……251
  - 案例 8-6：数据整理……251
  - 8.2.3 回测分析……262
  - 案例 8-7：回测分析……262
  - 8.2.4 专业回报分析……268
  - 案例 8-8：量化交易回报分析……268
- 8.3 完整的 LSTM 量化分析程序……279
  - 案例 8-9：LSTM 量化分析程序……280
  - 8.3.1 数据整理……280
  - 8.3.2 量化回测……284
  - 8.3.3 回报分析……285
  - 8.3.4 专业回报分析……288

## 第 9 章 日线数据回溯分析
- 9.1 数据整理……293
  - 案例 9-1：数据更新……294
  - 案例 9-2：数据整理……296
- 9.2 回溯分析……307
  - 9.2.1 回溯主函数……307
  - 9.2.2 交易信号……308
- 9.3 交易接口函数……309
  - 案例 9-3：回溯分析……309
  - 案例 9-4：多模式回溯分析……316

## 第 10 章 Tick 数据回溯分析
- 10.1 Fin 金融模块库……318
  - 案例 10-1：Fin 功能展示……318
  - 案例 10-2：量化交易回报分析……330
  - 案例 10-3：完整的量化分析程序……343
- 10.2 Tick 分时数据量化分析……357
  - 案例 10-4：Tick 分时量化分析程序……357

总结……371
附录 A TensorFlow 1.1 函数接口变化……372
附录 B 神经网络常用算法模型……377
附录 C 机器学习常用算法模型……414

## 第 1 章 TensorFlow 概述

TensorFlow 是谷歌公司推出的一个划时代的神经网络、深度学习开发平台。TensorFlow 是一个庞大的系统，结构复杂，功能强大。如图 1-1 所示是 TensorFlow 项目的 Logo。

TensorFlow 是谷歌发布的第二代机器学习系统，是一个利用数据流图（Data Flow Graphs）进行数值计算的开源软件库。数据流图中的节点（Node）代表数学运算操作；边（连线，Edge）表示节点之间相互流通的多维数组，即张量（Tensor）。

![](img/78eb676c584abe249cda782e9c1dd99b_17_0.png)

TensorFlow 具有灵活的架构：

- 可以让用户将计算部署在台式机、服务器或者移动设备的一个或多个 CPU 上，而且无须重写代码。
- 系统内置自动求导（Auto-differentiation）算法，可用于各种基于梯度的神经网络模型中。
- 通过灵活的 Python 接口，在 TensorFlow 中表达算法模型将更为简单。

TensorFlow 最初是由 Google Brain（谷歌大脑）团队开发出来的，目的是用于进行机器学习和深度神经网络的研究，但该系统的通用性使其广泛应用于多个领域中。目前 Google 内部已大量使用与 TensorFlow 相关的 AI（人工智能）技术，包括 Google App 的语音识别、Gmail 的自动回复、Google Photos 的图片搜索等功能。

### 1.1 TensorFlow 要点概括

为了让广大初学者，特别是金融量化、数据分析领域的非 IT 人员，能够快速掌握 TensorFlow 的全貌，我们特对 TensorFlow 的要点进行了简单概括，如下所示。

（1）TensorFlow 是一个神经网络、人工智能开发平台，内置了 TensorBoard 可视化智能数据分析软件。

（2）TensorFlow 有两种分析（预测）模式，即数值分析（回归）和数据分类。

（3）TensorFlow 有三大简化常用接口，分别是 TFlearn、TensorLayer 和 Keras。无论神经网络模型的形态如何变化，结构如何复杂、抽象，其目的就是预测与分类。无论 TensorFlow 的形态如何复杂，我们只需要使用简化的工程 API 接口，直接调用即可。

### 1.2 TensorFlow 简化接口

TensorFlow 功能强大，使用灵活，能够根据数据流图实现多种专业的神经网络模型。但在 TensorFlow 功能强大的背后，需要用户熟悉神经网络模型理论知识，通晓 TensorFlow 各种复杂的算法。

为方便用户操作，在发布 TensorFlow 的同时，谷歌采用了多种措施来降低 TensorFlow 的学习难度，简化用户接口。

此外，第三方也基于 TensorFlow 系统，推出了多种简化版本的 TensorFlow 接口，其中比较有名的有：

- Keras，专业深度学习框架，其设计参考了 Torch，用 Python 语言编写，是一个高度模块化的神经网络库，支持 GPU 和 CPU。Keras 2 与 TensorFlow 的兼容性得到进一步强化。此外，新版本的 TensorFlow 也内置了 Keras 模式的 API 接口。
- TensorLayer，基于 TensorFlow 开发的深度学习与强化学习库。它提供了高级别的深度学习 API，不仅可以加快研究人员的试验速度，也能够减少实际开发当中的重复工作。TensorLayer 非常易于修改和扩展，这使它可以用 于机器学习的研究与应用。
- TFlearn，一个模块化和透明的深度学习库，构建在 TensorFlow 之上。它为 TensorFlow 提供了高层次的 API，目的是便于快速搭建试验环境，同时保持对 TensorFlow 的完全透明和兼容性。

在这三者当中，Keras 是老牌的深度学习架构，后端同时支持多种系统，比如 Theano、TensorFlow、PyTorch 等。

TensorLayer 是最新的 TensorFlow 简化接口，其发展迅猛，源码架构简单，只有一个源码文件夹。但是 TensorLayer 封装的神经网络模型较少，而且对于 summary 日志数据支持不灵活，只能是 log 目录，无法自定义。

本书将重点介绍以上三种简化 API 接口的案例，让很多的初学者能够快速上手 TensorFlow 这个强大专业的神经网络、深度学习架构。

由于 TensorFlow 发展很快，以上几种简化 API 接口的更新也非常频繁，经常出现版本冲突。为了尽量避免这种情况，不影响大家的实际编程，我们会对以上程序接口进行二次封装，统一采用 Top-AI（TAI）极宽版本的 AI 函数接口。

熟悉了 TensorFlow 简化接口后，大家再进一步学习 TensorFlow 底层函数时会更加轻松，也会更加容易理解相关的算法模型。

### 1.3 Keras 简介

本书尽量以 Keras 简化接口为主，也会适当调用 TFlearn、TensorLayer、sklearn 等系统的工具函数，这些都是常用的编程模式。

本书以 Keras 简化接口为主，主要基于以下几个原因。

- Keras 后端同时支持 Theano、TensorFlow、PyTorch、MXNet、CNTK 等多种深度学习、神经网络系统。
- Keras 模型结构简单、清晰，特别适合初学者和非专业人员使用，对于金融量化、数据分析从业人员而言，是首选的入门教程。
- Keras 2 与 TensorFlow 高度融合，TensorFlow 内部也提供了 Keras 兼容模式的接口。
- Keras 2 内置的序列模型（Sequential）、函数模型（Functional）结构，可以与 TensorFlow 的数据流图运算、PyTorch 的函数运算完美对接。

虽然 Keras 使用简单、方便，但其在存在一些缺点，主要表现为：

- 运行速度慢，同样的模型比 TensorFlow 等编程慢 1 倍左右。
- 系统封闭，不便于优化模型内部参数。

笔者认为，这两点对于非专业人员，特别是金融量化行业的人员而言，不是主要问题。

对于运行速度慢这个问题，可以通过增加硬件设备来解决。神经网络更多的是借助于 GPU 等加速卡进行运算的，而且 Keras 2 和 TensorFlow 等系统本身也在不断优化运行速度。通常初学者可以直接使用现有的神经网络模型，而不需要自己进行底层参数优化。

### 1.4 运行环境模块的安装

TensorFlow 和配套的 GPU 开发环境的安装及配置需要一系列的步骤，非常烦琐，在此我们只是简单介绍 CUDA 运行环境的安装。

#### 1.4.1 CUDA 运行环境的安装

安装 CUDA 运行环境，一路选择默认设置，按回车键就可以了。安装成功后，CUDA 8 的目录结构如图 1-2 所示。

![](img/78eb676c584abe249cda782e9c1dd99b_20_0.png)

安装完毕后，进入 DOS 命令窗口，运行如下命令：

```
nvcc -V
```

如果 CUDA 8 安装成功，则会输出如下信息：

```
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Mon Jan 9 17:32:33 CST 2017
Cuda compilation tools, release 8.0, V8.0.60
```

在上面命令中，nvcc 是 NVIDIA 开发的编译工具，通过命令行选项可以在不同阶段启动不同的工具完成编译工作，其目的在于隐藏复杂的 CUDA 编译细节。

参数 -V 要大写，可以显示 nvcc 工具的版本信息，通常用于检查 CUDA 开发环境是否安装成功。

### 案例 1-1：重点模块版本测试

案例 1-1 文件名称是 kc101ver.py，主要用于测试重点模块版本。
案例 1-1 的源码很简单，需要注意的是源码头部的 import 语句（用于导入模块）。

```python
import tensorflow as tf
import tensorlayer as tl
import keras as ks
import nltk

import pandas as pd
import tushare as ts
import matplotlib as mpl
import plotly
import arrow

#import tflearn
tflearn = tf.contrib.learn
```

前面的 import 语句用于导入常用的模块。请注意模块的缩写字符，以及最后两行语句：

```python
#import tflearn
tflearn = tf.contrib.learn
```

TFLearn 模块有两种导入方式。严格说来，第二种方式不是标准的导入模块命令，而是一种别名方式，是长变量名称的缩写。

这是因为 TFLearn 本身是 TensorFlow 系统内置的第三方模块，目前也有独立版本的 TFLearn 模块，不过两者的源码并非完全同步，笔者在测试 tf-gpu 1.2rc1 和 tflearn 0.31 时就发现有版本冲突问题。

为了程序的兼容性，通常采用以下方式：

```python
tflearn = tf.contrib.learn
```

案例 1-1 的核心代码也很简单，就是输出相关模块的版本号。

```python
#1
print('\n#1 tensorflow.ver:', tf.__version__)

#2
print('\n#2 tensorlayer.ver:', tl.__version__)

#3
print('\n#3 keras.ver:', ks.__version__)

#4
print('\n#4 nltk.ver:', nltk.__version__)

#5
print('\n#5 pandas.ver:', pd.__version__)

#6
print('\n#6 tushare.ver:', ts.__version__)

#7
print('\n#7 matplotlib.ver:', mpl.__version__)

#8
print('\n#8 plotly.ver:', plotly.__version__)

#9
print('\n#9 arrow.ver:', arrow.__version__)

#10
print('\n#10 tflearn.ver:')
```

需要注意的是，在上面代码中使用的版本显示命令是：

`__version__`

这虽然不是标准的 Python 语法，但是一种潜在的约定，也被称为魔法语句。其对应的输出信息是：

```
Using TensorFlow backend.
#1 tensorflow.ver: 1.3.0
#2 tensorlayer.ver: 1.6.1
#3 keras.ver: 2.0.7
#4 nltk.ver: 3.2.4
#5 pandas.ver: 0.20.3
#6 tushare.ver: 0.8.7
#7 matplotlib.ver: 2.0.2
#8 plotly.ver: 2.0.15
#9 arrow.ver: 0.10.0
#10 tflearn.ver:
```

需要注意第一条输出信息：

```
Using TensorFlow backend.
```

此输出信息表示使用 TensorFlow 作为后端程序，这是使用 import 语句导入 Keras 模块时自动输出的信息。这条信息只在第一次运行程序时出现，大家可以多运行几次看看效果。

此外，还需要注意最后一条输出信息：

```
#10 tflearn.ver:
```

TFLearn 没有版本号，这是因为 TFLearn 模块不支持相关的版本命令。

TFLearn 原本是 TensorFlow 内置的模块，所以没有加入相关的版本语句，即使我们采用 import 语句导入独立的 TFLearn 模块，也无法显示相关的版本号。

案例 1-1 测试了 TensorFlow 结合金融工程、量化回溯、数据分析等项目常用时常用的 Python 模块版本，其中：

- TensorFlow 是神经网络、深度学习开发平台。
- TF Learn、Keras、TensorLayer 是 TensorFlow 的简化接口。
- NLTK 是语义分析模块。
- Pandas 是新一代数据分析神器。
- Plotly 是新一代互动型数据可视化绘图神器。
- Arrow 是新一代优雅、简洁的时间模块。
- Matplotlib 是经典的绘图模块。
- TuShare 是国内股票数据的采集模块。

以上模块和对应的版本，我们均已集成在 zwPython 2017m10 版本当中，使用 Mac、Linux 等平台或者其他 Python 开发环境的用户，请自行安装升级以上模块，并注意其版本号。

在安装升级相关模块时，请注意其所依赖的模块软件，特别是在手动升级独立的安装包时更要注意。

数字货币量化项目的数据接口部分，建议使用 Github 项目网站上的 CCXT 模块库，该模块库提供了近百个主流的数字货币交易所 API 接口，包括数据下载、程序化交易，TopQuant.vip 极宽量化网站也有相关的介绍。

### 案例 1-2：GPU 开发环境测试

案例 1-2 文件名是 kc102gpu.py，主要用于测试 Python 的 GPU 开发环境是否安装成功。

因为 TensorFlow 模块本身兼容 CPU、GPU 平台，因此没有 GPU 卡的用户也可以运行本案例，只是输出数据略有不同。

案例 1-2 的主要源码如下：

```python
import tensorflow as tf

#-----------------
#1
print('\n#1')
cfg=tf.ConfigProto(log_device_placement=True)
hello = tf.constant('Hello, TF!')
```
