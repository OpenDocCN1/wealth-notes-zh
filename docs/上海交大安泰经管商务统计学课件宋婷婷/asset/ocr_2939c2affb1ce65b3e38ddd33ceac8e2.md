# ANOVA Table

| Source of Variation | Sum of Squares | Degrees of Freedom | Mean Square | F | p-Value |
|---|---|---|---|---|---|
| Treatments | SSTR | $k-1$ | $MSTR=\frac{SSTR}{k-1}$ | $\frac{MSTR}{MSE}$ |  |
| Error | SSE | $n_T-k$ | $MSE=\frac{SSE}{n_T-k}$ |  |  |
| Total | SST | $n_T-1$ |  |  |  |

SST is partitioned into SSTR and SSE.

SST's degrees of freedom $(df)$ are partitioned into SSTR's $df$ and SSE's $df$.

+   - ANOVA can be viewed as the process of partitioning the **total sum of squares** and the **d.f.** into corresponding sources: treatments and error.
+     （分̄边差̄边法！）
+   - Dividing the sum of squares by the appropriate d.f.’s provides the variance estimates.