# Sampling Distribution

+   It can be show that
+   
+   $\mu_W = \frac{1}{2} n_1 (n_1 + n_2 + 1), \qquad \sigma_W = \sqrt{\frac{1}{12} n_1 n_2 (n_1 + n_2 + 1)}$

+   For large samples (i.e., $n_1 \ge 7$ and $n_2 \ge 7$), we can use normal approximation.

+   In our example, $n_1 = 12$, $n_2 = 10$, $\mu_W = 138$, $\sigma_W = 15.1658$, so the p-value
+   
+   $p\text{-value} = 2 \times P\left(N(0,1) \ge \frac{169.5 - 0.5 - 138}{15.1658}\right) = 0.0378 < \alpha$
+   
+   and we reject $H_0$.