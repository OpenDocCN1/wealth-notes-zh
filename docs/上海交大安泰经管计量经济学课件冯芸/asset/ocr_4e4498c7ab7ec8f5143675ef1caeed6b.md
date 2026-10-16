# OLS的统计性质：一阶矩

+   - 线性：参数估计量 $\hat{\beta}_j$ 是 $y_i$ 的线性函数
+   
+     $$\hat{\beta}_j = \frac{\sum \hat{r}_{ji} y_i}{\sum \hat{r}_{ji}^2}$$

+   - 无偏性
+   
+     $$E(\hat{\beta}_j \mid \mathbf{X}) = \beta_j, \quad j = 0, \cdots, k$$
+   
+     $$E(\hat{\beta}_j) = \beta_j, \quad j = 0, \cdots, k$$