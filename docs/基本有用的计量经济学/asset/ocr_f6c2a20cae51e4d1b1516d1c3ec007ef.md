的，从而证明个体没有精确控制断点能力。

下面进入第二步，估计在断点处的因果效应。可以利用局部简单平均值法，类似于完全随机化实验的分析；也可以采用局部线性回归方法，两种方法都需要选择带宽，第三种方法是多项式回归，可以使用相对较大的带宽或适用于整个样本。另一个相关的是核函数的选择，Imbens and Lemieux(2008)指出矩形核函数所得到的结果与其他核函数得到的结果没有太大差异，由于矩形核比较简单，这里便使用矩形核，对所有个体施加相同的权重。使用用户写的命令 rdbwselect 进行最优带宽选择，该命令提供了 Ludwig and Miller(2007)的交叉验证方法(VC)、Imbens and Kalyanaraman(2012)的带宽选择方法(IK)和 Calonico et al.(2014a)的带宽选择方法(CCT)三种最优带宽选择方法，我们选择交叉验证方法，利用下列命令估计最优带宽，得到的最优带宽为 0.28。

```
rdbwselect vote margin, c(0) kernel(uni) bwselect(CV) cvgrid_min(0.1) cvgrid_max(0.5) cvplot①
```

选项 c(0)表示断点为 0，kernel(uni)可以选择核类型，这里使用矩形核，bwselect()用于选择最优带宽估计方法，包括 CV、IK、CCT 三种方法可供选择，我们使用 CV，cvgrid_min(#)和 cvgrid_max 来限制画出交叉验证目标函数时的起始和结束位置，使用 cvplot 画出交叉验证目标函数图，运行上述命令后，会显示出图 9.8，利用另外两种方法会得到更小的带宽。下面，我们将带宽限制在 0.28 范围内，利用局部平均值法和局部线性回归法估计 RDD 因果效应参数。利用(9.15)估计下列回归：

```
keep if margin >= -0.28 & margin <= 0.28
gen d = margin > 0
eststo m1: reg vote 1.d, vce(robust)
eststo m2: reg vote d##c.margin, vce(robust)
esttab m1 m2 using llr.rtf, star(* .10 * * .05 * .01) nogap nonumber replace ///
mtitle("Local Average" "LLR") se(%5.4f) ar2 keep(1.d margin 1.d#c.margin _cons)
```

表 9.2 是上述程序估计出的回归结果，d 的系数是 RDD 估计量，第 1 列为局部平均估计量，即两组结果平均值之差，为 0.19。图 9.12 显示，局部平均显示不是很好的估计，图中显示在断点附近，结果变量与参考变量之间呈现正向斜率，因而局部线性回归可能是更好的选择。表 9.2 第 2 列是相应的局部线性

> ① 该命令为老版命令，通过 ssc install rdrobust, replace 或 findit rdrobust 安装使用。