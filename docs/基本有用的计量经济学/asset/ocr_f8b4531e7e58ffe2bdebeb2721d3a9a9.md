# Step 2: Test of balancing property of the propensity score

Use option detail if you want more detailed output

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

The balancing property is satisfied

This table shows the inferior bound, the number of treated and the number of controls for each block

| Inferior of block of pscore | treat 0 | treat 1 | Total |
| .2 | 156 | 82 | 238 |
| .4 | 84 | 93 | 177 |
| .6 | 9 | 10 | 19 |
| Total | 249 | 185 | 434 |

Note: the common support option has been selected

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

End of the algorithm to estimate the pscore

* * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * * *

首先，它会给出干预变量列表，提示利用逻辑回归估计倾向指数模型，然后提醒公共区间为[0.2379,0.6756]，接着对拟合的倾向指数进行简单统计，列出分布信息。下面进入第一步，识别最优分层数，因为没有加可选项detail，所以直接报告最终得到的最佳分层数为4层。然后，进入第二步倾向指数的平衡指数特征检验，显示满足平衡指数特征，说明倾向指数模型是充分的，可以作为倾向指数匹配的依据了。加上detail 会显示详细的平衡指数特征检验过程，为节约空间，在此省略。最后，将满足共同区间的分层信息显示出来，包括3层，[0.2,0.4]、[0.4,0.6]及0.6以上，有一层不满足共同区间要求，没有报告。

在估计出倾向指数之后，即可以采用相应的匹配命令进行倾向指数匹配了，我们只列出分层匹配命令的语法，其他命令语法相似。

```
atts outcome treatment [if exp] [in range], pscore(scorevar)
    blockid(blockvar) [ comsup detail bootstrap bs_options ]
```

Options

pscore(varname) is a compulsory option that specifies the name of the user-provided variable containing the estimated propensity score.

blockid(varname) is a compulsory option that specifies the name of the user-provided variable containing the block identifier of the estimated propensity score.

comsup restricts the computation of the ATT to the region of common support.