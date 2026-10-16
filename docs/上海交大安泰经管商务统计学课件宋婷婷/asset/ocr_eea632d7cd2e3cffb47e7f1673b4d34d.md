# Type I Error Rates for Multiple Comparisons

+   - One approach for controlling α_EW is the Bonferroni adjustment—the overall α_EW=0.05.
+   - How?
+   - We simply adjust the comparisonwise error rate α = α_EW / C_k².

| k | C_k² | α_EW (α=0.05) | α_EW (α=0.05 / C_k²) |
|---|---|---|---|
| 3 | 3 | 0.1426 | 0.0492 |
| 5 | 10 | 0.4013 | 0.0489 |
| 10 | 45 | 0.9006 | 0.0488 |
| 20 | 190 | 0.9999 | 0.0488 |

+   - **Criticism:** The correction comes at the cost of increasing the probability of producing false negatives, i.e., increasing type 2 error β.