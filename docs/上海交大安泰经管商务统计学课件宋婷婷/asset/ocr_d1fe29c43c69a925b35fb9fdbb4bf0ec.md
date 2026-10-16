# The F Test

+   - If the null hypothesis is true and the ANOVA assumptions are valid, the sampling distribution of MSTR/MSE is an F-distribution $(k-1, n_T - k)$. See Appendix
+   - Step 3: A random variable of F distribution with parameters $d_1$ and $d_2$ arises as the ratio of two independent (scaled) chi-squared random variables:
+     $X \sim F(d_1, d_2) \Rightarrow X = \frac{U_1/d_1}{U_2/d_2} = \frac{SSTR/(k-1)}{SSE/(n_T - k)} = \frac{MSTR}{MSE}$
+     where $U_i \sim \chi^2(d_i), i = 1,2.$
+   - If the means of the $k$ populations are not equal, the value of MSTR/MSE will be inflated because MSTR overestimates $\sigma^2$.
+   - Hence, we will reject $H_0$ if the resulting value of MSTR/MSE appears to be too large to have been selected at random from the appropriate F distribution.