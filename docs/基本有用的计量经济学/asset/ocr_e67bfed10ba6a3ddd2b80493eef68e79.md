# 第五节 断点回归设计的软件实现

在 Stata 软件中，断点回归设计的估计很简单，可以使用线性回归命令进行估计。下面我们使用 Lee（2008）的数据 lee.dta 为例，估计在位党的竞选优势。数据 lee.dta 包括两个主要变量：vote 是民主党本次的选票份额，margin 是民主党上次竞选中所获选票边际，即民主党选票份额减去共和党选票份额。margin 是参考变量，如果 margin 大于 0，民主党就是在位党，这是一个精确断点回归设计，我们关心在位党是否因为在位而获得更多的选票，是否具有在位优势。与 Lee and Lemieux(2010)一致，我们将样本限制在民主党上次选票边际在 ±0.5 之间，共 4900 个样本点。

首先画出结果变量与参考变量之间的关系图，选择带宽 0.01，共 100 个区间，可以利用 egen 的 cut()函数实现这一点：

```
use lee, clear
keep if margin<= .5&margin>= -.5 // 限制样本范围
local h = 0.01 // 改变带宽
egen group = cut(margin), at(- .50('h').50) // 根据参考变量，划分成相同距离区间
collapse (count) n = vote (mean) vote margin,by(group) // 按区间计算平均值，并计算每个区间中的个体数
gen x = -.5 + 1/_N * _n - 1/(_N * 2) // 参考变量区间中点
gen x2 = x^2
gen x3 = x^3
gen x4 = x^4
* 利用 4 阶多项式拟合
reg vote x x2 x3 x4 if x<0
predict votel if e(sample)
reg vote x x2 x3 x4 if x>= 0
predict voter if e(sample)
* 画出结果变量（vote）与参考变量（margin）关系图
sc vote x, xline(0) || line votel x || line voter x, legend(off) ///
xlabel(-0.5(0.1)0.5) xtitle("Democratic Vote Share Margin of Victory, Election t") ///
note(Note: Replicate figure 7 of Lee and Lemieumx (2010))
```