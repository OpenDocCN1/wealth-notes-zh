```
Out[32]: [200.0, 200.0, 200.0, 200.0, 200.0, 200.0, 200.0, 200.0, 200.0, 200.0]

In [33]: sum(res) / len(res)
Out[33]: 200.0
```

这是一个用来与其他更复杂的方法做比较的强有力的基准。

# 9.4 神经网络智能体

CartPole 游戏也可以被转换为一个分类问题：环境状态由 4 个特征值组成，对应每组给定特征值的正确操作是需要预测的标签。通过与环境交互，神经网络智能体可以收集由特征值和标签组合组成的数据集。给定这个不断增长的数据集，可以训练神经网络来学习给定环境状态的正确动作。在这种情况下，神经网络代表策略，智能体会根据新体验更新策略。

首先是一些库的引用。

```
In [34]: import tensorflow as tf
     from keras.layers import Dense, Dropout
     from keras.models import Sequential
     from keras.optimizers import Adam, RMSprop
     from sklearn.metrics import accuracy_score
     Using TensorFlow backend.
```

```
In [35]: def set_seeds(seed=100):
     random.seed(seed)
     np.random.seed(seed)
     tf.random.set_seed(seed)
     env.seed(seed)
     env.action_space.seed(100)
```

然后是一个 NNAgent 类，它结合了智能体的主要元素：策略的神经网络模型、根据策略选择动作、更新策略（训练神经网络），以及多个回合的学习过程本身。智能体同时使用探索和利用来选择一个动作。探索是指随机动作，独立于当前策略。利用是指从当前策略派生的动作。这么做是因为某种程度的探索可以确保持获得更丰富的经验，从而使智能体的学习得到改善。

```
In [36]: class NNAgent:
     def __init__(self):
         self.max = 0
         self.scores = list()
         self.memory = list()
         self.model = self._build_model()

     def _build_model(self): 
         model = Sequential()
         model.add(Dense(24, input_dim=4,
                 activation='relu'))
         model.add(Dense(1, activation='sigmoid'))
         model.compile(loss='binary_crossentropy',
```