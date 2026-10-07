素，即：

$S = \frac{1}{T}\sum_{i=1}^{T}\sum_{j=1}^{T}\hat{\varepsilon}_i\hat{\varepsilon}_j\boldsymbol{x}_i\boldsymbol{x}_j'$ \tag{2.62}

式（2.62）看似合理，但因其存在两个问题，所以并不正确：（1）该表达式中一共有$T^2$项求和，而求和项之前的比例系数仅仅是$1/T$，因此$S$可能不收敛；（2）即使$S$收敛，它也很有可能不是正定的，从而使得$\boldsymbol{b}$的协方差矩阵非正定，这显然有违常理。

为了解决上述问题，Newey and West（1987）给出了在同时考虑$\varepsilon$的自相关和异方差时，中间矩阵$Q$的相合估计量：

$S = \frac{1}{T}\sum_{t=1}^{T}\hat{\varepsilon}_t^2\boldsymbol{x}_t\boldsymbol{x}_t' + \frac{1}{T}\sum_{j=1}^{J}\sum_{t=j+1}^{T}w_j\hat{\varepsilon}_t\hat{\varepsilon}_{t-j}(\boldsymbol{x}_t\boldsymbol{x}_{t-j}' + \boldsymbol{x}_{t-j}\boldsymbol{x}_t')$ \tag{2.63}

其中 $w_j = 1 - \frac{j}{1+J}$

比较式（2.60）和式（2.63）不难发现，后者中的第一项正好对应仅有异方差的情况，而第二项则是针对自相关性的修正。其中$J$是计算自相关性影响的最大滞后阶数（Newey and West 1994给出了自动计算$J$取值的自适应算法），$w_j$是滞后期$j$的权重系数，由其表达式不难看出自相关性的影响随着滞后期$j$的增大而减小。将式（2.63）中的$S$代入式（2.56），得到协方差矩阵的Newey–West异方差自相关相合估计量：

$\hat{V}_{\mathrm{OLS}} = T(\boldsymbol{X}'\boldsymbol{X})^{-1}\boldsymbol{S}(\boldsymbol{X}'\boldsymbol{X})^{-1}$ \tag{2.64}

将式（2.64）的协方差矩阵对角线上的元素求平方根，就得到回归系数的标准误。该估计量在实证资产定价研究中应用非常广泛，几乎任何一篇分析因子或者异象的论文，在检验收益率显著性时，都会提到诸如“经Newey–West调整后的$t$-值（Newey–West adjusted $t$-statistic）”或者“经Newey–West调整后的标准误（Newey–West adjusted standard error）”。这些描述的含义是使用式（2.64）计算回归系数的标准误以及$t$-值²。

再回到本节异象检验的问题中，按照如下步骤即可在OLS回归时对回归系数的标准误进行Newey–West调整：

（1）使用异象收益率作为被解释变量、多因子中$K$个因子的收益率以及一个截距项（一共$K+1$项）作为解释变量，进行时序回归OLS估计，得到残差$\hat{\varepsilon}$。

（2）使用$K+1$个解释变量$X$和残差$\hat{\varepsilon}$，根据式（2.63）和式（2.64）计算出经Newey–West调整后的$\hat{V}_{\mathrm{OLS}}$，计算时依照Newey and West（1994）给出的公式确定最大滞后期数$J$: