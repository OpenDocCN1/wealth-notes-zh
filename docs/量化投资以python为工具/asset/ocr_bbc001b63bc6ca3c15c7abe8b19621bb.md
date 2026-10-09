# 17.1.2 最小平方法

在上述的线性模型中，$X_i$ 和 $Y_i$ 是样本数据，但还需要知道 $\alpha$ 与 $\beta$ 的值。对于 $\alpha$ 与 $\beta$ 的值，可以根据 $X_i$ 和 $Y_i$ 的数据来估计。估计的方法有很多种，比如前面讲过的矩估计法就是其中一种。很明显，采取不同的估计方法，就会得到不同的 $\alpha$ 与 $\beta$ 的估计值。而我们想要的自然是最接近真实值的估计，那么，应该采取什么方法进行估计呢？构建线性回归模型的目的是要来解释 $Y$ 的变动，因此我们认为 $\alpha$ 与 $\beta$ 的估计式是令 $Y_i$ 中未被 $X_i$ 解释的部分 $\varepsilon_i$ 越小越好。我们可以将误差项 $\varepsilon_i$ 加总令其最小，但是，误差 $\varepsilon_i$ 有时大于零有时小于零，加总时会正负抵消，这样就无法区分没有偏差和正负偏差相抵消时的情况。因此，我们选择使用误差 $\varepsilon_i$ 之平方和并令其最小，这种估计方法被称为最小平方法（Least Squares Method）。求解 $\alpha$ 和 $\beta$ 的方式可以用数学公式表达如下：

$$\min_{\alpha, \beta} Q(\alpha, \beta) = \frac{1}{n} \sum_{i=1}^{n} (Y_i - \alpha_i - \beta_i X_i)^2$$

根据函数最小化原则，能够令误差平方和 $Q(\alpha, \beta)$ 最小的 $\alpha$、$\beta$ 必须满足以下一阶条件（First Order Conditions）：

$$\frac{\partial Q(\alpha, \beta)}{\partial \alpha} = -2\frac{1}{n} \sum_{i=1}^{n} (Y_i - \alpha_i - \beta_i X_i) = 0 \tag{17.1}$$

$$\frac{\partial Q(\alpha, \beta)}{\partial \beta} = -2\frac{1}{n} \sum_{i=1}^{n} (Y_i - \alpha_i - \beta_i X_i) X_i = 0$$

根据这两条一阶条件公式，可以得到 $\alpha$ 与 $\beta$ 的最小平法估计式（Estimator）：

$$\hat{\beta} = \frac{\sum_{i=1}^{n} (X_i - \bar{X})(Y - \bar{Y})}{\sum_{i=1}^{n} (X_i - \bar{X})^2} \tag{17.2}$$

$\hat{\alpha} = \bar{Y} - \hat{\beta} \bar{X}$

其中 $\bar{X}$ 和 $\bar{Y}$ 为 $X$ 和 $Y$ 的样本均值，注意式(17.2)是估计式，也就是说，样本观测值不同，我们就会得到不同的 $\hat{\alpha}$、$\hat{\beta}$。如果我们将样本观测值 $x_1, x_2, \dots, x_n$ 和 $y_1, y_2, \dots, y_n$ 代入估计式(17.2)中，即可得到 $\hat{\alpha}$ 与 $\hat{\beta}$ 之最小平方估计值（Estimate）：

$$\hat{\beta}_n = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n} (x_i - \bar{x})^2}$$

$\hat{\alpha}_n = \bar{y} - \hat{\beta} \bar{x}$

其中 $\bar{x}$、$\bar{y}$ 是 $\bar{X}$、$\bar{Y}$ 的实现值。

将 $\hat{\alpha}$ 与 $\hat{\beta}$ 之最小平方估计值代入线性模型，可得由样本观测值 $x_1, x_2, \dots, x_n$ 解释的 $\hat{y}_i$：