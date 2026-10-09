# ### 3.1 不确定性与风险

金融理论的核心就是当前不确定性和风险条件下的投资、交易和估值。本节会以尽量正式的形式介绍这些主题的核心概念，而重点是从概率论角度介绍支撑数量金融的基本概念。

# #### 3.1.1 定义

假设一种经济模式只能观察到两个时间点：今天，记为 $t=0$；一年后，记为 $t=1$。本章后面的内容在很大程度上就是基于这样的**静态经济模式**。

在 $t=0$ 时，没有任何不确定性。在 $t=1$ 时，经济模式可以是有限状态集合 $\Omega$ 中的一个状态 $S$，$\omega \in \Omega = \{\omega_1, \omega_2, \cdots, \omega_S\}$。$\Omega$ 被称为**状态空间**，它的基数（或者势）为 $S$，$[2] = S$。

代数 $\mathscr{F}$ 在 $\Omega$ 中是一个集合族，满足以下条件。

+   1. $\Omega \in \mathscr{F}$
2. $\mathbb{E} \in \mathscr{F} \Rightarrow \mathbb{E}^c \in \mathscr{F}$
3. $\mathbb{E}_1, \mathbb{E}_2, \cdots, \mathbb{E}_i \in \mathscr{F} \Rightarrow \bigcup_{i=1}^{i} \mathbb{E}_i \in \mathscr{F}$

$\mathbb{E}^c$ 是集合 $\mathbb{E}$ 的补集，幂集 $\wp(\Omega)$ 是最大的代数，同时 $\mathscr{F} = \{\emptyset, \Omega\}$ 是 $\Omega$ 中最小的代数。代数就是经济模式中**可观测事件**的一个模型。在此背景下，经济模式的单一状态 $\omega \in \Omega$ 可以解释为一个原子事件。

**概率**就是把一个实数 $0 \leqslant p_\omega = P(\{\omega\}) \leqslant 1$ 赋值给一个状态 $\omega \in \Omega$，或者把一个实数 $0 \leqslant P(\mathbb{E}) \leqslant 1$ 赋值给一个事件 $\mathbb{E} \in \mathscr{F}$。如果已知一个事件的所有状况的概率，则该事件的概率可以加总为 $P(\mathbb{E}) = \sum_{\omega \in \mathbb{E}} p_\omega$。

**概率测度** $P: \mathscr{F} \to [0, 1]$ 具有以下特征。

+   1. $\forall \mathbb{E} \in \mathscr{F}: P(\mathbb{E}) \geqslant 0$
2. $P(\bigcup_{i=1}^{i} \mathbb{E}_i) = \sum_{i=1}^{i} \mathbb{E}_i$，其中 $\mathbb{E}_i \in \mathscr{F}$
3. $P(\Omega) = 1$

将 $\{\Omega, \mathscr{F}, P\}$ 这 3 个要素组合在一起会形成一个**概率空间**。概率空间是现代经济学中对**不确定性**的正式表达形式。如果概率测度 $P$ 是固定的，就可以说这种经济模式是处于**风险**中的。如果经济模式中的所有经济主体都知道概率分布，那么这一经济模式就是**信息对称**的。

给定一个概率空间 $\{\Omega, \mathscr{F}, P\}$，**随机变量**是一个函数 $S: \Omega \to \mathbb{R}$，$\omega \mapsto S(\omega)$，并且该函数是 $\mathscr{F}$-可测的。这就意味着每一个 $\mathbb{E} \in [a, b]$，$a < b$ 都满足以下关系：

$S^{-1}(\mathbb{E}) = \{\omega \in \Omega : S(\omega) \in \mathbb{E}\} \in \mathscr{F}$

如果 $\mathscr{F} = \wp(\Omega)$，则随机变量的**期望值**会被定义为：

注 1：参见 Jacod and Protter (2004) 关于概率论的介绍。

注 2：在**动态经济模式**中，不确定性会随时间的流逝而逐渐消失，也就是说，在今天与一年后的今天之间的每一天流逝都不确定性减少。