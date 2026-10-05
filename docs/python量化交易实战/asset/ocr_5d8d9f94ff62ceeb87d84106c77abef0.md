[10. 4.]

这里 result.params 对应的是两个结果，第一个为 noise 值，第二個为对应的 weight 值。使用回归计算因子与收益率的代码段如下：

```
factor_1 = [6,1,2,7]
return_profit = [0.02,0.004,0.005,0.007]

x_ = sm.add_constant(factor_1)
y = return_profit

model = sm.OLS(y, x_)
results = model.fit()
print(results.params)
```

而最终打印结果如下：

[ 0.003  0.0015]

# 2. 计算下一期股票收益率

在得到了对应的回归系数后，下一步是计算对应的预期股票收益率。代码段如下：

```
pre_return_profit = results.params[0] + np.asarray(factor_1) * results.params[1]
```

其结果如下：

[ 0.012  0.0045  0.006  0.0135]

# 3. 计算预期收益率与真实收益率的相关系数

最后一步是计算预期收益率与真实收益率之间的相关系数。代码段如下：

```
corr = np.corrcoef(pre_return_profit,return_profit)
print(corr)
```

完整代码如【程序 6-4】所示。

# 【程序 6-4】

```
import statsmodels.api as sm
import numpy as np

factor_1 = [6,1,2,7]
return_profit = [0.02,0.004,0.005,0.007]

x_ = sm.add_constant(factor_1)
y = return_profit
model = sm.OLS(y, x_)
```