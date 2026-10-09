```
elif reward == 1: ⑩
done = False
elif (self.accuracy < self.min_accuracy and
      self.bar > self.osm + 10): ⑪
done = True
else: ⑫
done = False
state = self._get_state()
info = {}
return state, reward, done, info
```

+   ① 定义所需的最低准确率。
② 选择用于定义金融市场状态的数据。
③ 将环境重置为其初始值。
④ 检查智能体是否选择了正确的动作（成功交易）。
⑤ 定义智能体收到的奖励。
⑥ 将奖励添加到总奖励中。
⑦ 使环境向前运行一步。
⑧ 计算给定所有步骤（交易）的成功的动作（交易）的准确率。
⑨ 如果智能体到达数据集的末尾，则成功。
⑩ 如果智能体采取了正确的动作，那么它可以继续前进。
⑪ 如果在一些初始步骤之后，准确率下降到最低水平以下，则该回合结束（失败）。
⑫ 对于其余情况，智能体可以继续前进。

Finance 类的实例表现得就像 OpenAI Gym 的环境。特别是，在这种基本情况下，该实例表现得与 CartPole 环境完全一样。

```
In [55]: env = Finance('EURE', 'EUR') ①

In [56]: env.reset()
Out[56]: array([1.819 , 1.8579, 1.7749, 1.8579])

In [57]: a = env.action_space.sample()
Out[57]: 0

In [58]: env.step(a)
Out[58]: ([array([1.8579, 1.7749, 1.8579, 1.947 ])], 0, False, {})
```

+   ① 指定用于定义代表状态数据的交易标的的代号和特征类型（交易标的的代号或对数收益率）。为 CartPole 游戏开发的 DQLLagent 能否学习在金融市场中进行交易？是的，它能，如下面的代码所示。然而，尽管智能体在训练回合中提高了其交易技能（平均而言），但结果并不“惊艳”（参见图 9-3）。