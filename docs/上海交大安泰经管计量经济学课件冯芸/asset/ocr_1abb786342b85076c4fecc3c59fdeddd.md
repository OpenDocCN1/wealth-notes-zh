对变换后的模型 运用OLS法，即
\[ \min_{\hat{\beta}_0,\hat{\beta}_1} \sum (u_i^*)^2 = \min_{\hat{\beta}_0,\hat{\beta}_1} \sum (y_i^* - \beta_0 x_{0i}^* - \beta_1 x_{1i}^*)^2 \]
\[ = \min_{\hat{\beta}_0,\hat{\beta}_1} \sum \frac{1}{h_i} (y_i - \beta_0 - \beta_1 x_i)^2 \]
\[ = \min_{\hat{\beta}_0,\hat{\beta}_1} \sum \frac{1}{h_i} u_i^2 \]
权系数 $w_i = \frac{1}{h_i}$