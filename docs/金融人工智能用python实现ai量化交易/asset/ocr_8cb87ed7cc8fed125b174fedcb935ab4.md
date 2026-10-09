```
```python
def _get_state(self):
    return self.data[self.features].iloc[self.bar -
        self.lags:self.bar]

def seed(self, seed):
    random.seed(seed)
    np.random.seed(seed)

def reset(self):
    self.treward = 0
    self.accuracy = 0
    self.performance = 1
    self.bar = self.lags
    state = self.data[self.features].iloc[self.bar - self.lags:self.bar]
    return state.values

def step(self, action):
    correct = action == self.data['d'].iloc[self.bar] ⑧
    ret = self.data['r'].iloc[self.bar] * self.leverage ⑧
    reward_1 = 1 if correct else 0
    reward_2 = abs(ret) if correct else -abs(ret) ⑦
    factor = 1 if correct else -1
    self.treward += reward_1
    self.bar += 1
    self.accuracy = self.treward / (self.bar - self.lags)
    self.performance *= math.exp(reward_2) ⑧
    if self.bar >= len(self.data):
        done = True
    elif reward_1 == 1:
        done = False
    elif (self.performance < self.min_performance and
            self.bar > self.lags + 5):
        done = True
    else:
        done = False
    state = self._get_state()
    info = {}
    return state.values, reward_1 + reward_2 * 5, done, info
```
```

+   ① 定义状态的特征。
② 要使用的滞后期长度。
③ 所需的最低总收益。
④ 添加金融衍生变量（简单移动平均线、动量、滚动波动率）。
⑤ 数据的高斯归一化。
⑥ 该步的杠杆收益率。
⑦ 该步的基于收益率的奖励。
⑧ 该步结束后的总收益。

新的 Finance 类为金融市场环境的建模提供了更大的灵活性。以下代码是一个有两个特征和 5 个滞后项的示例。