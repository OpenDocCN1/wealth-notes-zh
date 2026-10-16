# Example: A Survey about Dining Hall –Analysis

+   - Test Statistics
+     $F = MSTR/MSE = 392.56/3.14 = 124.84$

# ANOVA Table

| Source of Variance | Sum of Squares | Degree of Freedom | Mean Square | F | P-value | F crit |
|---|---|---|---|---|---|---|
| Treatments | 785.11 | 2 | 392.56 | 124.84 | 4.54E-43 | 3.02 |
| Blocks | 44.00 | 4 | 11.00 |  |  |  |
| Error | 1289.19 | 410 | 3.14 |  |  |  |
| Total | 2118.31 | 416 |  |  |  |  |

+   - Rejection Rule
+     - p-value Approach: Reject $H_0$ if p-value $\le .05$
+     - Critical Value Approach: Reject $H_0$ if F $\ge$ 3.02, where the value of $F_{.05}$ = 3.02 is based on an F distribution with 2 numerator d.f. and 410 denominator d.f.

+   - Conclusion
+     - With F = 124.84, we reject $H_0$ and conclude that there is a significant difference between the students' evaluations of the three dining halls.