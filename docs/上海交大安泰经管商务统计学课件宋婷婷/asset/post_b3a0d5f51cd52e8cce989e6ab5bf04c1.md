
# Multiple Comparison Procedures

- Suppose that ANOVA(MSE,1/n=1/n) has provided statistical evidence to reject the null hypothesis of equal population means.
- H₀: μᵢ = μ⁰ for all i and Hₐ: μᵢ ≠ μ⁰ for some i.
- Fisher's least significant difference (LSD) procedure is a two-step procedure for pairwise comparisons of several treatment groups, which helps to determine where the differences occur.
  - Perform an ANOVA test (global), if rejected at the pre-specified level of significance
  - Perform all pairwise comparisons at the same level of significance.
- There are two variations of the second procedure, based on two different test statistics:
  - t-statistics t = (x̄ᵢ - x̄ⱼ) / √[MSE(1/nᵢ + 1/nⱼ)]
  - Sample mean differences: x̄ᵢ - x̄ⱼ

# Fisher’s LSD Procedure: t-statistics

- Step 1: Hypotheses
  H0: μi = μj
  Ha: μi ≠ μj
- Step 3: Test Statistic (two-sample mean test with unknown equal variance)
  t(nT − k) = (x̄i − x̄j) / √[MSE(1/ni + 1/nj)]
- Step 4: Rejection Rule
  - p-value Approach: Reject H0 if p-value ≤ α
  - Critical Value Approach: Reject H0 if t ≤ −tα/2 or t ≥ tα/2 where the value of tα/2 is based on t distribution (d.f. = nT − k) Why?

# Fisher's LSD Procedure : x̄_i − x̄_j

- Step 1: Hypotheses
  H₀ : μ_i = μ_j
  Hₐ : μ_i ≠ μ_j
- Step 3: Test Statistic
  x̄_i − x̄_j
- Step 4: Rejection Rule
  - Reject H₀ if
    |x̄_i − x̄_j| ≥ LSD
  where
  LSD = t_{α/2}(n_T − k) √[MSE(1/n_i + 1/n_j)]

# Example: Reed Manufacturing – Fisher’s LSD Procedure

- **Empirical question:** Recall that Janet Reed wants to know if there is any significant difference in the mean number of hours worked per week for the department managers at her three manufacturing plants.
- In the first step, ANOVA has provided **statistical evidence** to reject the null hypothesis of equal population means.
- Fisher’s least significant difference (LSD) procedure (pairwise comparisons C₃² = 3) can be used to determine **where the differences occur**.
- For α = .05 and n_T − k = 15 − 3 = 12 degrees of freedom, t₀.₀₂₅ = 2.179
  $$\text{LSD}=t_{\alpha/2}\sqrt{\text{MSE}\left(1/n_i+1/n_j\right)}$$
  $$=2.179\sqrt{25.667\left(1/5+1/5\right)}=6.98$$

# Example: Reed Manufacturing – Fisher’s LSD Procedure

- LSD for Plants 1 and 2
- Hypotheses (A)
  H₀: μ₁ = μ₂
  Hₐ: μ₁ ≠ μ₂
- Rejection Rule
  Reject H₀ if |x̄₁ − x̄₂| ≥ 6.98
- Test Statistic Rule
  |x̄₁ − x̄₂| = |55 − 68| = 13
- Conclusion
  The mean number of hours worked at Plant 1 is not equal to the mean number worked at Plant 2.

# Example: Reed Manufacturing – Fisher’s LSD Procedure

- LSD for Plants 1 and 3
  - Hypotheses (B)
    - $H_0: \mu_1 = \mu_3$
    - $H_a: \mu_1 \neq \mu_3$
  - Rejection Rule
    - Reject $H_0$ if $|\bar{x}_1 - \bar{x}_3| \ge 6.98$
  - Test Statistic Rule
    - $|\bar{x}_1 - \bar{x}_3| = |55 - 57| = 2$
  - Conclusion
    - There is no significant difference between the mean number of hours worked at Plant 1 and the mean number of hours worked at Plant 3.

# Example: Reed Manufacturing – Fisher’s LSD Procedure

- LSD for Plants 2 and 3
  - Hypotheses (C)
    H₀: μ₂ = μ₃
    Hₐ: μ₂ ≠ μ₃
  - Rejection Rule
    Reject H₀ if |x̄₂ − x̄₃| ≥ 6.98
  - Test Statistic Rule
    |x̄₂ − x̄₃| = |68 − 57| = 11
  - Conclusion
    The mean number of hours worked at Plant 2 is not equal to the mean number worked at Plant 3.

# Type I Error Rates for Multiple Comparisons

- In our example, we used Fisher’s LSD procedure to make **three pairwise comparisons**, each with a significance level α.
- What is the probability that we commit a Type I error on **at least one** of the three tests?
- The **comparisonwise Type I error rate** α indicates the level of significance associated with a single pairwise comparison.
- The **experimentwise Type I error rate** αₑw rate is the probability of making a Type I error on at least one of the \(C_k^2 = \frac{k!}{2!(k-2)!}\) pairwise comparisons.

\[\alpha_{EW}=1-(1-\alpha)^{C_k^2}\]

- The experimentwise Type I error rate gets larger for problems with more treatments (larger k).
