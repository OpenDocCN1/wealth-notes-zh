和投资组合层面生成一系列实时更新的因子暴露度报告,以更好地应对市场环境的变化。

# 表 15-1 因子归因模型

| 主动收益归因 | 计算公式 |
|---|---|
| 因子 $f_1$ | $\sum_i w_{a,i} \beta_{i,1} f_1$ |
| ⋮ | ⋮ |
| 因子 $f_k$ | $\sum_i w_{a,i} \beta_{i,k} f_k$ |
| 特殊收益 | $\sum_i w_{a,i} \alpha_i$ |
| 主动风险归因 | |
| 因子 $f_1$ | $\left(\sum_i w_{a,i} \beta_{i,1}\right)^2 \mathrm{Var}(f_1)$ |
| ⋮ | ⋮ |
| 因子 $f_k$ | $\left(\sum_i w_{a,i} \beta_{i,k}\right)^2 \mathrm{Var}(f_k)$ |
| 特殊风险 | $\left(\sum_i w_{a,i}\right)^2 \mathrm{Var}(\alpha_1)$ |
| 因子协方差调整项 | $2 \sum_m \sum_n \left(\sum_i w_{a,i} \beta_{i,m} \sum_j w_{a,j} \beta_{j,n}\right) \mathrm{Cov}(f_m , f_n)$ |

# 本章小结

本章试图向读者展示基本面量化投资的全过程：如何利用阿尔法模型估计收益，如何利用风险模型估计风险，如何利用优化模型生成投资组合中的个股权重，如何通过业绩归因模型分解主动收益和主动风险的来源。通过这样的流程，量化基金经理便能够将一个好的投资想法实施到投资组合中，最终获取超额收益。