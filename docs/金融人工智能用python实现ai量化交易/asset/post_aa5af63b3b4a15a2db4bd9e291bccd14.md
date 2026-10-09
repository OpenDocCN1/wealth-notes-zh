
In [76]: accurancy_score(np.sign(test[tags:]), np.sign(y))
Out[76]: 0.6708428246013668

- 1. 将数据拆分为训练数据子集和测试数据子集。
- 2. 在训练数据上拟合模型。
- 3. 在测试数据上测试模型。

### 8.5 金融特征

RNN 的应用不仅限于原始价格或收益率数据，还可以包括附加特征以改进它的预测性能。以下 Python 代码向数据集中添加了典型的金融特征。

```python
In [77]: data = generate_data()

In [78]: data['r'] = np.log(data / data.shift(1))

In [79]: window = 20
     data['mom'] = data['r'].rolling(window).mean()
     data['vol'] = data['r'].rolling(window).std()

In [80]: data.dropna(inplace=True)
```

- 1. 增加时间序列动量特征。
- 2. 增加滚动波动率特征。

#### 8.5.1 估计

在估计任务中，样本外准确率可能会显著下降，这有些出乎意料。换句话说，在这种特殊情况下添加金融特征并没有观察到任何改进。

```python
In [81]: split = int(len(data) * 0.8)

In [82]: train = data.iloc[:split].copy()

In [83]: mu, std = train.mean(), train.std()

In [84]: train = (train - mu) / std

In [85]: test = data.iloc[split:].copy()

In [86]: test = (test - mu) / std

In [87]: g = TimeseriesGenerator(train.values, train['r'].values,
                     length=lags, batch_size=5)

In [88]: set_seeds()
     model = create_rnn_model(hu=100, features=len(data.columns),
              layer='SimpleRNN')

In [89]: %time
        model.fit_generator(g, epochs=100, steps_per_epoch=10,
                       verbose=False) 
CPU times: user 5.24 s, sys: 1.08 s, total: 6.32 s
Wall time: 2.73 s

Out[89]: <keras.callbacks.callbacks.History at 0x7ff76f313c950>

In [90]: g_ = TimeseriesGenerator(test.values, test['r'].values,
         length=lags, batch_size=5) 

In [91]: y = model.predict(g_, flatten()) 

In [92]: accuracy_score(np.sign(test['r'].iloc[lags:]), np.sign(y)) 
Out[92]: 0.37299771167048057
```

- 1. 计算训练数据的一阶矩和二阶矩。
- 2. 对训练数据应用高斯归一化。
- 3. 对测试数据应用高斯归一化（基于来自训练数据的统计数据）。
- 4. 在训练数据上拟合模型。
- 5. 在测试数据上测试模型。

### 8.5.2 分类

迄今为止的分析都在使用 Keras 中的 RNN 模型进行估计，以预测金融工具价格的未来方向。我们所关心的问题可能会更好地直接转换为分类问题。以下 Python 代码会处理二进制标签数据并直接预测价格变动的方向。这次我们使用 LSTM 层，即使对于相对少量的隐藏单元和有限的几个训练轮数，样本外的准确率也相当高。该方法通过适当调整类权重来解决类别不平衡的问题，在这种情况下，预测准确率非常高，约为 65%。

```python
In [93]: set_seeds()
        model = create_rnn_model(hu=50,
                     features=len(data.columns),
                     layer='LSTM',
                     algorithm='classification')

In [94]: train_y = np.where(train['r'] > 0, 1, 0) 

In [95]: np.bincount(train_y)
Out[95]: array([2374, 1142])

In [96]: def cw(a):
            c0, c1 = np.bincount(a)
            w0 = (1 / c0) * (len(a)) / 2
            w1 = (1 / c1) * (len(a)) / 2
            return {0: w0, 1: w1}

In [97]: g = TimeseriesGenerator(train.values, train_y,
            length=lags, batch_size=5)

In [98]: %%time
model.fit_generator(g, epochs=5, steps_per_epoch=10,
        verbose=False, class_weight=cw(train_y))
CPU times: user 1.25 s, sys: 159 ms, total: 1.41 s
Wall time: 947 ms

Out[98]: <keras.callbacks.callbacks.History at 0x7ff4f34bf90>

In [99]: test_y = np.where(test['r'] > 0, 1, 0)

In [100]: g_ = TimeseriesGenerator(test.values, test_y,
                 length=lags, batch_size=5)

In [101]: y = np.where(model.predict(g_, batch_size=None) > 0.5, 1, 0).flatten()

In [102]: np.bincount(y)
Out[102]: array([492, 382])

In [103]: accuracy_score(test_y[lags:], y)
Out[103]: 0.6498855835240275
```

- ❶ 用于分类的 RNN 模型。
- ❷ 二元训练标签。
- ❸ 训练标签的组频率。
- ❹ 二进制的测试标签。

### 8.5.3 深度RNN

最后，本章将演示深度 RNN，它是具有多个隐藏层的 RNN。如同创造深度 DNN 一样简单，唯一的要求是对于非最终隐藏层，参数 return_sequences 需要被设置为 True，以下用于创建深度 RNN 的 Python 函数还添加了 Dropout 层以避免潜在的过拟合，预测准确率与上一节中的预测准确率相当。

```python
In [104]: from keras.layers import Dropout

In [105]: def create_deep_rnn_model(hl=2, hu=100, layers='SimpleRNN',
    optimizer='rmsprop', features=1,
    dropout=False, rate=0.3, seed=100):
        if hl <= 2: hl = 2 ❶
        if layers == 'SimpleRNN':
            layer = SimpleRNN
        else:
            layer = LSTM
        model = Sequential()
        model.add(layer(hu, input_shape=(lags, features),
                     return_sequences=True
                 )) ❷
        if dropout:
            model.add(Dropout(rate, seed=seed)) ❸
        for _ in range(2, hl):
            model.add(layer(hu, return_sequences=True))
        if dropout:
            model.add(Dropout(rate, seed=seed)) ❹
        model.add(layer(hu)) ❺
        model.add(Dense(1, activation='sigmoid'))
        model.compile(optimizer='rmsprop',
        loss='binary_crossentropy',
        metrics=['accuracy'])
        return model

In [106]: set_seeds(1)
model = create_deep_rnn_model(
hl=2, hu=50, layers='SimpleRNN',
features=len(data.columns),
dropout=True, rate=0.3)

In [107]: %time
model.fit_generator(g, epochs=200, steps_per_epoch=10,
verbose=False, class_weight=cw(train_y))
CPU times: user 14.2 s, sys: 2.85 s, total: 17.1 s
Wall time: 7.09 s

Out[107]: <keras.callbacks.callbacks.History at 0x7fe7ef6428790>

In [108]: y = np.where(model.predict(g, batch_size=None) > 0.5, 1, 0).flatten()

In [109]: np.bincount(y)
Out[109]: array([550, 324])

In [110]: accuracy_score(test_y[lags:], y)
Out[110]: 0.6430205949655751
```

- ① 保证最少有两个隐藏层。
- ② 第一个隐藏层。
- ③ Dropout 层。
- ④ 最终隐藏层。
- ⑤ 建立分类模型。

## 8.6 结论

本章基于 Keras 库介绍了 RNN 模型，并说明了这种神经网络在金融时间序列数据中的应用。从 Python 语言的角度看，使用 RNN 与使用 DNN 并无太大区别。一个主要区别是训练数据和测试数据必须以序列形式呈现给各自的方法，但是通过应用 TimeseriesGenerator 函数可以轻松实现这一点，该函数使用的生成器对象会将序列数据转换为 Keras 中的 RNN 能处理的数据。

本章中的示例适用于金融价格序列和金融收益率序列。此外，我们还可以轻松添加金融特征，比如时间序列动量。为模型创建提供的函数可以使用 SimpleRNN 层或 LSTM 层以及不同的优化器等，还可以在浅层神经网络和深层神经网络的背景下对估计和分类问题进行建模。

在预测市场方向时，分类实例得到的样本外预测准确率相对较高，但对估计实例而言并没有那么高，甚至可能很低。

# 第9章 强化学习

像人类一样，我们的智能体能够自己学习以实现成功的策略，从而获得最大的长期回报。这种通过完全基于自奖励或惩罚的试错学习的范式被称为强化学习。¹

> ——DeepMind，2016 年

第 7 章和第 8 章中应用的学习算法属于**监督学习**的范畴，这些方法要求有一个包含特征和标签的数据集，此数据集能够支持这些算法学习特征和标签之间的关系，从而在估计或分类任务中取得成功。正如第 1 章中的简单示例所示，**强化学习**（reinforcement learning, RL）的工作方式与此不同。它不需要预先给出一个全面的特征和标签数据集，数据是由学习智能体在与感兴趣的环境交互时生成的。本章详细介绍了强化学习，说明了其基本概念，以及该领域最流行的算法之一：**Q 学习**。神经网络不会被强化学习算法取代，在本章的讨论范畴内它们通常也发挥着重要作用。

- 9.1 节解释了强化学习中的基本概念，比如环境、状态和智能体。9.2 节介绍了强化学习环境的 OpenAI Gym 套件，其中是以 CartPole 环境作为示例的。第 2 章简要介绍和讨论过在这种环境中，智能体必须学习如何通过推车轮向左或向右移动来平衡推车轮上的杆子。9.3 节展示了如何使用隆维和蒙特卡罗模拟来解决 CartPole 问题。DNN 等标准监督学习算法通常不适合解决像 CartPole 这样的问题，因为它们缺乏延迟奖励的概念，9.4 节会对此进行讨论。9.5 节讨论了 DQL 智能体，该智能体明确地考虑了延迟奖励并能够解决 CartPole 问题。9.6 节将相同的智能体应用于简单的金融市场环境中，尽管智能体在这一场景中表现不佳，但该示例表明 DQL 智能体可以学习交易并成为通常所说的**交易机器人**。为了改进 DQL 智能体的学习，9.7 节提出了一个改进的金融市场环境，除其他好处外，它允许使用一种以上的特征来描述环境状态。基于这种改进的环境，9.8 节引入并应用了一种改进的 Q 学习金融智能体，作为交易机器人，它的表现更加出色。

注 1：参见“Deep Reinforcement Learning”。

## 9.1 基本概念

本节简要概述了强化学习中的基本概念，包括以下几种。

- **环境**
**环境**定义了当前的问题，可以是要玩的计算机游戏或要进行交易的金融市场。

- **状态**
**状态**包含描述环境当前状态的所有相关参数。在计算机游戏中，这可能是整个屏幕及其像素。在金融市场中，这可能包括当前和历史价格水平或金融指标，比如移动平均线、宏观经济变量等。

- **智能体**
**智能体**这个词包含了与环境交互并且对这些交互中学到的强化学习算法的所有元素。在计算机游戏中，智能体可能代表玩游戏的玩家。在金融环境中，智能体可以代表在市场中进行交易的交易者。

- **动作**
智能体可以从一组（有限的）被允许的动作中选择一个**动作**。在计算机游戏中，被允许的动作可能是向左或向右移动，而在金融市场中，被允许的动作可能是做多或做空。

- **步骤**
给定智能体的动作，环境状态会被更新，这样的更新通常被称为一个**步骤**。步骤的概念可以包含两个步骤之间的相同或者不同的时间间隔。虽然在计算机游戏中，与游戏环境的实时交互是通过相当短且相同的时间间隔（“游戏时钟”）来模拟的，但诸如与金融市场环境交互的交易机器人则可以在更长且不同的时间间隔内采取动作。

- **奖励**
根据智能体选择的动作，对其实行**奖励**（或惩罚）。对于计算机游戏，积分是一种典型的奖励。在金融环境中，利润（或亏损）是一种标准的奖励（或惩罚）。

- **目标**
**目标**是指智能体试图最大化的内容。在计算机游戏中，这通常是智能体达到的分数。对于金融交易机器人，这可能是累积的交易利润。

- **策略**
**策略**定义了智能体在给定环境状态下所采取的动作。给定计算机游戏的特定状态（由构成当前场景的所有像素表示），策略可能会指定智能体选择“向右移动”作为动作。观察到连续 3 个价格上涨的交易机器人可能会根据其策略决定做空市场。

- **回合**
**回合**是从环境的初始状态成功或可预见的失败的一组步骤。在游戏中，这是从游戏开始到输或赢为止。以金融界为例，这就是从年初到年底，或者到破产。

Sutton 和 Barto（2018）介绍了强化学习这一领域，该书详细讨论了上述概念，并在大量具体示例的基础上对它们进行了说明。接下来的几节再次选择了一种实用的、以应用为导向的强化学习方法，所讨论的示例会基于 Python 代码进一步说明上述所有概念。

## 9.2 OpenAI Gym

在第 2 章介绍的大多数成功范例中，强化学习起着主导作用，这激发了人们对强化学习算法的广泛兴趣。OpenAI 是一个致力于促进人工智能研究，特别是强化学习研究的组织。OpenAI 开发并开源了一套环境，称为 OpenAI Gym，允许通过标准化 API 训练 RL 智能体。

在众多环境中，有模拟经典强化学习问题的 CartPole 环境（或游戏），即把一根杆子直立在手车上，目的是通过左右移动推车来学习平衡杆子的策略。环境状态由 4 个参数表示，包括以下物理测量值：推车位置、推车速度、极角和极角速度（尖端）。图 9-1 描述了一个可视化的环境。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_227_0.png)

考虑以下 Python 代码，该代码实例化了 CartPole 的环境对象并检查了观察空间。观察空间是环境状态的模型。

```python
In [1]: import os
   import math
   import random
   import numpy as np
   import pandas as pd
   from pylab import plt, mpl
   plt.style.use('seaborn')
   mpl.rcParams['savefig.dpi'] = 300
   mpl.rcParams['font.family'] = 'serif'
   np.set_printoptions(precision=4, suppress=True)
   os.environ['PYTHONHASHSEED'] = '0'

In [2]: import gym
In [3]: env = gym.make('CartPole-v0')
In [4]: env.seed(100)
   env.action_space.seed(100)
Out[4]: [100]
In [5]: env.observation_space
Out[5]: Box(4,)
```
