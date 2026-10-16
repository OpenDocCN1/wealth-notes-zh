# Testing the difference between Two Population Proportions

+   - Intuitively, we would reject $H_0$ if $\bar{p}_1 - \bar{p}_2$ is sufficiently large.
+   - To derive a test statistic for the hypothesis tests, we need the sampling distribution of $\hat{p}_1 - \hat{p}_2$.
+   - As in the 1-sample tests, we shall use normal approximation.
+   - Step 3: If $H_0$ is true and $p_1 = p_2 = p$, then

$$\frac{\hat{p}_1 - \hat{p}_2}{\sqrt{p(1-p)\left(\frac{1}{n_1}+\frac{1}{n_2}\right)}} \sim N(0,1)$$

where $p$ can be estimated by the pooled estimation

$$\bar{p} = \frac{n_1\bar{p}_1 + n_2\bar{p}_2}{n_1 + n_2}$$