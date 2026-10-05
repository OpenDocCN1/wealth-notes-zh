# 3.1 SQN指数&策略评估参数

在讲解量化策略的编写之前，先介绍一些量化策略的评估参数。

量化交易有一套自己的评估指标参数的体系，用于考核量化策略的优劣。

常见的评估量化策略的指标参数如下。

+   - ROI：资产回报率。
- Starting Portfolio Value：起始资金。
- Final Portfolio Value：资产总值。
- Total Profit：利润总额。
- SharpeRatio：夏普指数。
- max_drawdown_len：最大回撤周期。
- max_drawdown：最大回撤。
- max_drawdown_money：最大回撤资金。

还有一个专业评估指数：SQN指数，这是量化策略评判指数，可以用来衡量交易系统好坏的程度。

SQN指数的英文全称是：System Quality Number，它也被称为“系统质量指数”，计算公式为：

SQN=（平均获利/标准差）×年交易次数的平方根

当交易数量≥30时，SQN指数通常被认为是可靠的。

SQN指数评判量化系统的标准如下。

+   - SQN指数=1.6～1.9，不怎么样，可以凑合用。
- SQN指数=2.0～2.4，普通。
- SQN指数=2.5～2.9，好。
- SQN指数=3.0～5.0，杰出。
- SQN指数=5.1～6.9，一流。