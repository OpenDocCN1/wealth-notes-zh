```
Data columns (total 6 columns):
#	Column	Non-Null Count	Dtype
0	EUR_USD	340 non-null	float64
1	r	340 non-null	float64
2	s	340 non-null	float64
3	m	340 non-null	float64
4	v	340 non-null	float64
5	d	340 non-null	int64
dtypes: float64(5), int64(1)
memory usage: 18.6 KB
```

```
In [61]: ax = learn_env.data[learn_env.sym].plot(figsize=(10, 6))
plt.axvline(learn_env.data.index[-1], ls='--')
valid_env.data[learn_env.symbol].plot(ax=ax, style='--')
plt.axvline(valid_env.data.index[-1], ls='--')
test_env.data[learn_env.symbol].plot(ax=ax, style='--');
```

![](bbox=[0.15, 0.36, 0.88, 0.66])

基于 Oanda 环境，可以对第 11 章中的交易机器人进行训练和验证。下面的 Python 代码会执行此任务并可可视化性能结果（参见图 12-4）。

```
In [62]: import sys
			sys.path.append('../../ch11/')
```

```
In [63]: import tradingbot
			Using Tensorflow backend.
```

```
In [64]: tradingbot.set_seeds(100)
			agent = tradingbot.TradingBot(24, 0.001, learn_env=learn_env, valid_env=valid_env)
```