另外，干扰项 $\boldsymbol { \varepsilon } _ { i }$ 的方差 $\sigma _ { 0 } ^ { 2 }$ 的估计式 $\hat { \sigma } ^ { 2 } = \frac { 1 } { n - 2 } \sum _ { i = 1 } ^ { n } \hat { \varepsilon } _ { i } ^ { 2 }$ 满足以下分布：

$$\begin{array} { r } \frac { ( n - 2 ) \hat { \sigma } ^ { 2 } } { \sigma _ { 0 } ^ { 2 } } \sim \chi ^ { 2 } ( n - 2 ) \end{array}$$

从 $\hat { \alpha } , \hat { \beta }$ 的均值和方差可以看出， $\hat { \alpha }$、 $\hat { \beta }$ 具有如下性质。

+   (1) 随着样本量 $n$ 的增加，估计式的精确度也会随之增加。每增加一个观测样本点，模型的精确度将会提高。

+   (2) $\sum _ { i = 1 } ^ { n } ( X _ { i } - \bar { X } ) ^ { 2 }$ 在 $\hat { \alpha }$、 $\hat { \beta }$ 的方差估计式中都有出现，且都在分母当中，说明 $\sum _ { i = 1 } ^ { n } ( X _ { i } - \bar { X } ) ^ { 2 }$ 越大， $\hat { \alpha }$、 $\hat { \beta }$ 方差越小，估计式的精确度增加。 $\sum _ { i = 1 } ^ { n } ( X _ { i } - \bar { X } ) ^ { 2 }$ 表示 $X$ 变量样本与样本的离散程度。也就是说当样本在 $X$ 取值上越离散，估计式的方差越小；在 $X$ 取值上越集中时，方差越大。

+   (3) $\chi ^ { 2 }$ 代表数据点相对于 $Y$ 轴的平均偏离程度，当 $\bar { X }$ 离 $Y$ 轴越远，回归估计线的截距项 $\hat { \alpha }$ 越不容易确定， $\hat { \alpha }$ 的精确度就会随之降低。

# 17.4 显著性检验

回想一下前面介绍的假设检验，对于一个随机变量，可以对其期望是否等于某个值进行检验。在回归分析中，我们往往会对估计出的系数进行假设检验。通常我们是对回归系数的真实值 $\alpha _ { 0 }$、 $\beta _ { 0 }$ 提出原假设，然后构建计算统计量的取值，并根据 $\hat { \alpha }$、 $\hat { \beta }$、 $\hat { \sigma } ^ { 2 }$ 的分布性质判断根据样本观测点 $x _ { i }$、 $y _ { i }$ 计算出来的 $\hat { \alpha } _ { n }$、 $\hat { \beta } _ { n }$ 是否显著异于0，以此来判断是否接受原假设。也正因为如此，我们把这样的假设检验叫作显著性检验，即变量之间的关系是否显著。以系数 $\hat { \beta }$ 为例，显著性检验分为如下4个步骤。

+   (1) 提出原假设 $H _ { 0 }$ ： $\beta _ { 0 } = b$ ；备择假设 $H _ { 1 }$ ： $\beta _ { 0 } \neq b$;

+   (2) 确定显著性水平0.05、0.01等（根据具体情况而定）；

+   (3) 构建、计算统计量 $t _ { \beta }$ —标准化的 $\hat { \beta } - b$ :

$$\begin{array} { r } { t _ { \beta } = \frac { \hat { \beta } - b } { \hat { \sigma } } } \ { = \frac { \hat { \beta } - b } { \sqrt { \sum _ { i = 1 } ^ { n } ( x _ { i } - \bar { x } ) ^ { 2 } } } = \frac { \hat { \beta } - b } { \sigma _ { 0 } } \bigg / \sqrt { \frac { ( n - 2 ) \hat { \sigma } ^ { 2 } / \sigma _ { 0 } ^ { 2 } } { n - 2 } } } \end{array}$$

式中 $\sqrt { \sum _ { i = 1 } ^ { n } ( X _ { i } - \bar { X } ) ^ { 2 } } \left( \frac { \hat { \beta } - b } { \sigma _ { 0 } } \right) ^ { 2 } \sim N ( 0 , 1 ) , ~ \frac { ( n - 2 ) \hat { \sigma } ^ { 2 } } { \sigma _ { 0 } ^ { 2 } } \sim \chi ^ { 2 } ( n - 2 )$ ，由此可得 $t _ { \beta }$ 服从 $t$ 分布：

$$\begin{array} { r } { t _ { \beta } \sim t ( n - 2 ) } \end{array}$$

+   (4) 查自由度 $d f = n - 2$ 的 $t$ 分布表得到临界值或者是计算出 $t _ { \beta }$ 对应的 $p$ 值，做出是否