Python 与量化投资：从基础到实战

+   风险约束：二次约束，用以约束组合的风险。

+   跟踪误差约束：二次约束，用以控制组合的跟踪误差，与风险约束的区别在于这里使用的是主动权重。

# 6.10.2 优化器的 API 接口

在优矿专业版中有专门的优化器 API 接口，创建一个优化器对象：

```
UqerOptimizer(signal, construct_date, risk_model='short', benchmark_str='ZZ500', **kwargs)
```

该接口用于创建一个优化器对象，对其参数的说明如下。

+   signal(Series, dict, list, numpy.ndarray)：信号，可以是预期收益率。如果为 Series，那么索引为 str 格式的 secID，值为该期信号值；如果为 dict，那么键为 str 格式的 secID，值为该期信号值。

+   construct_date(str)：组合构建的日期，合理的格式为'YYYYMMDD'。

+   risk_model(str)：风险模型预测周期，可选的参数为'day'、'short'、'long'，默认为'short'。

+   benchmark_str(str)：指定基准，默认为中证 500 指数，允许的值包括'ZZ500'、'HS300'、'SH50'、'SH180'和'SZZS'。

+   assets(Series, dict 或者 list)：用于导入上期的持仓权重，主要用于换手率约束当中。如果为 Series，那么索引为股票名称，值为上期权重；如果为 dict，那么键为股票名称，值为上期权重。

UqerOptimizer 的相关属性如下。

+   assets(DataFrame)：一个 index 为股票代码，列为'init_weights'、'min'、'max'的 DataFrame，用来记录初始权重和权重上下限约束。

+   optimal(bool)：记录组合优化的状态，默认为 False，优化成功则为 True。

+   construct_date：记录组合构建的日期。

+   risk_model_type(str)：记录风险模型存储的类型。