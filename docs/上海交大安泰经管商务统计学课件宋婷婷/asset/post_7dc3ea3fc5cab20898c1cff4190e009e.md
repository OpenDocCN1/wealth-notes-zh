
## Randomized Block Design: ANOVA

- The model for a RBD takes the form

$$x_{ij}=\mu+\alpha_j+\beta_i+\epsilon_{ij},\quad j=1,\ldots,k;\ i=1,\ldots,b$$

where $x_{ij}$ is the observed value for treatment $j$ in block $i$ and $n_T=kb$.

- The sum of squares total (SST) is partitioned into three groups: sum of squares due to treatments (SSTR), sum of squares due to blocks (SSBL), and sum of squares due to error (SSE).

$$\mathrm{SST}=\mathrm{SSTR}+\mathrm{SSBL}+\mathrm{SSE}$$

- The total degrees of freedom, $n_T-1$, are partitioned such that $k-1$ degrees of freedom go to treatments, $b-1$ go to blocks, and $(k-1)(b-1)$ go to the error term.

## Randomized Block Design: ANOVA Table

| Source of Variation | Sum of Squares | Degrees of Freedom | Mean Square | F | p-Value |
|---|---|---|---|---|---|
| Treatments | SSTR | $k - 1$ | $\mathrm{MSTR} = \mathrm{SSTR} / (k - 1)$ | $\mathrm{MSTR} / \mathrm{MSE}$ |  |
| Blocks | SSBL | $b - 1$ | $\mathrm{MSBL} = \mathrm{SSBL} / (b - 1)$ |  |  |
| Error | SSE | $(k - 1)(b - 1)$ | $\mathrm{MSE} = \mathrm{SSE} / [(k - 1)(b - 1)]$ |  |  |
| Total | SST | $n_T - 1$ |  |  |  |

- $\mathrm{SST} = \sum_{i=1}^{b} \sum_{j=1}^{k} (x_{ij} - \bar{x})^2$
- $\mathrm{SSTR} = \sum_{i=1}^{b} \sum_{j=1}^{k} (\bar{x}_j - \bar{x})^2 = b \sum_{j=1}^{k} (\bar{x}_j - \bar{x})^2$
- $\mathrm{SSBL} = \sum_{i=1}^{b} \sum_{j=1}^{k} (\bar{x}_i - \bar{x})^2 = k \sum_{i=1}^{b} (\bar{x}_i - \bar{x})^2$
- $\mathrm{SSE} = \sum_{i=1}^{b} \sum_{j=1}^{k} (x_{ij} - \bar{x}_i - \bar{x}_j + \bar{x})^2 = \mathrm{SST} - \mathrm{SSTR} - \mathrm{SSBL}$

## Example: A Survey about Dining Hall – Analysis

- **Goal**: After excluding the error incurred by different classes, test whether there is a significant difference between students’ evaluations of the three dining halls.
- **Block**: Due to the homogeneous condition for a block, we can regard a class as a block.
- Let $X_{ijm}$ be the evaluation given by $m$th student for dining hall $j$ in block $i$ (class $i$), where $i = 1,2,\cdots,5$; $j = 1,2,3$ and $m = 1,2,\cdots,M_i$.

**Data Table (keep two digits):** Treatments (Dining halls)

| Block (Class) | Treatment 1 | Treatment 2 | Treatment 3 | Block Means | # of Students |
|---|---|---|---|---|---|
| 1 | $x_{111},\ldots,x_{11M_1}$ | ... | ... | 6.65 | 33 |
| 2 | ... | ... | ... | 6.40 | 16 |
| 3 | ... | ... | ... | 7.23 | 45 |
| 4 | ... | ... | ... | 7.24 | 27 |
| 5 | ... | ... | ... | 6.95 | 21 |
| Treatment Means | 5.09 | 7.61 | 8.22 |  |  |

- **Mean Squares Due to Treatments**
  - $\mathrm{SSTR} = 139 \sum_{j=1}^{3} (\bar{x}_j - \bar{x})^2 = 139(5.05 - 6.96)^2 + 139(7.61 - 6.96)^2 + 139(8.22 - 6.96)^2 = 785.11$, $df_{TR} = 3 - 1$
  - $\mathrm{MSTR} = 785.11/(3 - 1) = 392.56$
- **Mean Squares Due to Blocks (MSBL)**
  - $\mathrm{SSBL} = \sum_{i=1}^{5} \sum_{j=1}^{3} \sum_{m=1}^{M_i} (\bar{x}_{i.} - \bar{x})^2 = 44.00$, $df_{BL} = 5 - 1$
  - $\mathrm{MSBL} = 44.00/(5 - 1) = 11.00$
- **Mean Squares Due to Error (MSE)**
  - $\mathrm{SST} = \sum_{i=1}^{5} \sum_{j=1}^{3} \sum_{m=1}^{M_i} (x_{ijm} - \bar{x})^2 = 2118.31$, $df_T = 417 - 1$
  - $\mathrm{SSE} = \mathrm{SST} - \mathrm{SSTR} - \mathrm{SSBL} = 1289.19$, $df_E = df_T - df_{TR} - df_{BL} = 410$
  - $\mathrm{MSE} = 1289.19/410 = 3.14$

- **Test Statistics**
  - $F = \mathrm{MSTR}/\mathrm{MSE} = 392.56/3.14 = 124.84$

### ANOVA Table

| Source of Variation | Sum of Squares | Degrees of Freedom | Mean Square | F | P-value | F crit |
|---|---|---|---|---|---|---|
| Treatments | 785.11 | 2 | 392.56 | 124.84 | 4.54E-43 | 3.02 |
| Blocks | 44.00 | 4 | 11.00 |  |  |  |
| Error | 1289.19 | 410 | 3.14 |  |  |  |
| Total | 2118.31 | 416 |  |  |  |  |

- **Rejection Rule**
  - p-value Approach: Reject $H_0$ if p-value $\le .05$
  - Critical Value Approach: Reject $H_0$ if $F \ge 3.02$, where the value of $F_{.05} = 3.02$ is based on an F distribution with 2 numerator d.f. and 410 denominator d.f.

- **Conclusion**
  - With $F = 124.84$, we reject $H_0$ and conclude that there is a significant difference between the students’ evaluations of the three dining halls.

## Other Related Designs

- RBD reduces the residual error in an experiment by removing variability due to a single known and controllable nuisance variable.
- There are several other types of designs that utilize the blocking principle:
  - Latin Square Design (blocking 2 nuisance variables)
  - Graeco-Latin Square Design (blocking 3 nuisance variables)
- The RBD we have learnt is indeed a Randomized Complete Block Design, in which each block contains all the treatments.
- Sometimes it is not possible to run all the treatments in each block.
  - Randomized Incomplete Block Designs (balanced/unbalanced)
