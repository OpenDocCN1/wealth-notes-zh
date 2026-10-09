在一个只有有限数量的可能状态的简单环境中，Q 可以用一张表格表示，其中列出了每个状态－动作组合的相应值。然而，在更有趣的设置中（如 CarPole 环境），状态数量过多，无法通过穷尽的方式写出 Q，因此，Q 通常被理解为一个函数。

这就是神经网络发挥作用的地方。在现实的设置和环境下，函数 Q 的封闭形式可能不存在，或者利用动态规划方法很难推导。因此 Q 学习算法通常仅以近似值为目标。神经网络具有通用逼近能力，是完成 Q 函数逼近的自然选择。

Q 学习的另一个关键要素是重复。DQL 智能体会重复许多经验（状态－动作组合）以定期更新策略函数 Q，这可以大大提高学习效果。此外，下面介绍的 DQL 智能体（DQLAgent）于学习过程中在探索和利用之间交替。交替会以系统的方式进行，因为智能体仅从探索开始（一开始它不可能学到任何东西），然后会缓慢但稳定地降低探索率 ε 直到达到最低水平。

```
```In [44]: from collections import deque
    from keras.optimizers import Adam, RMSprop

In [45]: class DQLAgent:
    def __init__(self, gamma=0.95, hs=24, opt=Adam,
            lr=0.001, f_finish=False):
        self.f_finish = f_finish
        self.epsilon = 1.0 ◊
        self.epsilon_min = 0.01 ◊
        self.epsilon_decay = 0.995 ◊
        self.gamma = gamma ◊
        self.batch_size = 32 ◊
        self.max_treward = 0
        self.averages = list()
        self.memory = deque(maxlen=2000) ◊
        self.osn = env.observation_space.shape[0]
        self.model = self._build_model(hu, opt, lr)

    def _build_model(self, hu, opt, lr):
        model = Sequential()
        model.add(Dense(hu, input_dim=self.osn,
                activation='relu'))
        model.add(Dense(hu, activation='relu'))
        model.add(Dense(env.action_space.n, activation='linear'))
        model.compile(loss='mse', optimizer=opt(lr=lr))
        return model

    def act(self, state):
        if random.random() <= self.epsilon:
            return env.action_space.sample()
        action = self.model.predict(state)[0]
        return np.argmax(action)```
```

> 注 3：实现类似于博客文章 “Deep Q-Learning with Keras and Gym” 中的发现。