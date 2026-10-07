下面仍然以单因子这种最简单的情况来画图，说明上述时间序列回归得到的 $E[R_i^e]$ 和 $\beta_i$ 的截面关系长什么样子。图2.4中的直线为 $E[R_i^e]=\beta_i\lambda$：当 $\beta_i=0$ 时，$E[R_i^e]=0$；此外，如果用该模型解释因子投资组合本身（即将因子投资组合视作一个资产放在截面关系式的左侧），且根据定义因子投资组合在自身上的暴露为1（即 $\beta_f=1$），因而有 $\lambda=0+1\times\lambda$。以上论述说明，时间序列回归得到的 $E[R_i^e]=\beta_i\lambda$ 这条直线一定会经过 $(0,0)$ 和 $(1,\lambda)$ 两点。

![](bbox=[0.15, 0.18, 0.85, 0.43])

图2.4

时序回归示意图

图2.4中所有黑色的实心圆点代表着资产，空心的点代表着因子投资组合。时间序列回归得到的多因子模型 $E[R_i^e]=\beta\lambda$ 就是经过原点和空心点的那条直线。所有资产到这条直线的距离就是资产的 $\hat{\alpha}_i$。需要特别强调的是，在使用时间序列回归时，需对每个资产 $i$ 分别独立用多因子模型进行时序回归。因此图2.4中的这条直线并不是以最小化 $\hat{\alpha}_i$ 的平方和为目的求出的，这是时序回归和本书2.2.2节介绍的截面回归的最大差别（截面回归是以最小化所有 $\hat{\alpha}_i$ 的平方和为目标的）。

有了时序回归模型，下一步就是计算各种参数的标准误，并进行检验。当随机扰动 $\varepsilon_{it}$ 不存在自相关（autocorrelation）或异方差（heteroskedasticity）时，时序回归参数的标准误可以由OLS的标准公式给出。进一步的，假设 $\varepsilon_{it}$ 满足IID正态分布，Michael Gibbons、Stephen Ross以及Jay Shanken在Gibbons et al.（1989）一文 中给出了检验 $\alpha_i$ 是否联合为零的方法。即便到了今天，该方法仍然是学术界检验和比较因子模型时的首选方法，由于影响深远，所以该方法也由三位教授姓氏的首字母命名为GRS检验。

在GRS检验中，原假设所有 $\alpha_i$ 均为零。定义向量 $\hat{\alpha}=[\hat{\alpha}_1,\hat{\alpha}_2,\cdots,\hat{\alpha}_N]'$ 以及 $\hat{\varepsilon}_t=$ $[\hat{\varepsilon}_{1t},\hat{\varepsilon}_{2t},\cdots,\hat{\varepsilon}_{Nt}]'$。GRS检验构建了如下满足自由度为 $T-N-K$ 和 $N$ 的 $F$ 分布的检验统计量（称为GRS test statistic）：