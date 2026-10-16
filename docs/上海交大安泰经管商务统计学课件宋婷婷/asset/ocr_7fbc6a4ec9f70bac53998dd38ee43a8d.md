# Randomized Block Design: ANOVA Table

| Source of Variation | Sum of Squares | Degrees of Freedom | Mean Square | F | p-Value |
|---|---|---|---|---|---|
| Treatments | SSTR | k - 1 | MSTR = SSTR / (k - 1) | MSTR / MSE | |
| Blocks | SSBL | b - 1 | MSBL = SSBL / (b - 1) | | |
| Error | SSE | (k - 1)(b - 1) | MSE = SSE / [(k - 1)(b - 1)] | | |
| Total | SST | n_T - 1 | | | |

+   - SST = Σ_{i=1}^{b} Σ_{j=1}^{k} (x_{ij} - x̄)^2
+   - SSTR = Σ_{i=1}^{b} Σ_{j=1}^{k} (x̄_j - x̄)^2 = b Σ_{j=1}^{k} (x̄_j - x̄)^2
+   - SSBL = Σ_{i=1}^{b} Σ_{j=1}^{k} (x̄_i - x̄)^2 = k Σ_{i=1}^{b} (x̄_i - x̄)^2
+   - SSE = Σ_{i=1}^{b} Σ_{j=1}^{k} (x_{ij} - x̄_i - x̄_j + x̄)^2 = SST - SSTR - SSBL