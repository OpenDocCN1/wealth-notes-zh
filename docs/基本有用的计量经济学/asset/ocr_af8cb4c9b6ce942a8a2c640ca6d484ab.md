| re78 | Coef. | AI Robust Std. Err. | z | P>|z| | [95% Conf. Interval] |
|------|-------|---------------------|---|------|----------------------|
| ATET |
| treat |
| (1 vs 0) | 2639.865 | 714.7758 | 3.69 | 0.000 | 1238.93 | 4040.799 |

倾向指数匹配完成后，可以使用辅助命令 teffects overlap 画出干预组和控制组的倾向指数分布图，以判断两组结果是否满足共同区间要求。

![](bbox=[0.155, 0.325, 0.815, 0.64])

# 图 6.1 teffects overlap 画出的倾向指数分布图

另一个官方的辅助命令是 tebalance(Stata 14 开始提供)，它主要用于检验协变量的平衡性，包括 4 个子命令，分别是 box、density、overid、summarize。tebalance box 可以画出匹配前后协变量的箱形图，只能在 nnmatch 和 psmatch 命令后面使用。tebalance density 画出两组个体的核密度分布图，teffects 子命令后均可使用。overid 平衡性检验只能在 ipw 命令后使用。summarize 可以计算标准化平均值差异和两组协变量的方差比，teffects 命令后均可以使用。

在前面的协变量匹配之后，可以用 tebalance summarize 计算所有协变量的标准化平均值差异和协变量方差比。