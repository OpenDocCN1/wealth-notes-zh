这里没有传入上一期权重，所以初始化了一个等权组合，如果要添加换手率约束，那么还需要创建优化器对象时传入上一期权重。

# 2. 权重分布图

这里展示了权重大于 0.00001 的股票的权重的分布：

```
import matplotlib.pyplot as plt
import seaborn as sns
from CAL.PyCAL import *
sns.set_style('white')
fig = plt.figure(figsize=(16,5))
ax = fig.add_subplot(111)
ax = weights.optimal_weights.plot(kind='bar', ax=ax)
ax.set_title(u'部分股票权重分布', fontproperties=font, fontsize=16)
ax.set_xlabel(u'股票代码', fontproperties=font, fontsize=13)
ax.set_ylabel(u'权重', fontproperties=font, fontsize=13)
ax.grid()
```

![](bbox=[0.145, 0.46, 0.855, 0.68])

# 3. 行业分布比较

因为我们在这里做的是行业中性，所以优化后组合的行业权重应与基准的行业权重一致：

```
# 获得基准成分股行业分类
equ_indu = DataAPI.EquIndustryGet(industryVersionCD=u"010303", secID=set_universe('HS300','20160930'), intoDate='20160930', field=u"secID, industryName1", pandas="1")
```