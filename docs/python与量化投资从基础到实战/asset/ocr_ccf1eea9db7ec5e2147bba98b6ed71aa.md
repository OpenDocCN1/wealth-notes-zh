| 属 性 | 类 型 | 注 释 |
|---|---|---|
| Alpha | list | 策略 CAPM 阿尔法 |
| beta | list | 策略 CAPM 贝塔 |
| sharpe | list | 策略年化夏普率 |
| volatility | list | 策略年化波动率 |
| max_drawdown | list | 策略最大回撤 |
| information_coefficient | list | 信息系数 |
| information_ratio | list | 信息比率 |
| turnover_rate | list | 换手率 |

其用法为在运行完策略后，在 code 单元中输入 perf 并运行，然后查看结果。

# 5.5 策略的风险评价指标

通过利用策略的风险评价指标，我们可以在各个维度都对策略有客观、全面的认识。优矿回测框架提供了如下风险指标。

# 1. 年化收益率（Annualized Returns）

表示投资期限为一年的预期收益率，计算公式为
$p_r = \left(\frac{p_{end}}{p_{start}}\right)^{(250/n)} -1$
其中，$p_{end}$ 指策略最终总资产，$p_{start}$ 指策略初始总资产，$n$ 指回测交易日数量。

# 2. 基准年化收益率（Benchmark Returns）

表示参考标准年化收益率，计算公式为
$B_r = \left(\frac{B_{end}}{B_{start}}\right)^{(250/n)} -1$
其中，$B_{end}$ 指基准最终值，$B_{star}$ 指基准初始值，$n$ 指回测交易日数量。