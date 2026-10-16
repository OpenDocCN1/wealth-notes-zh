
## Testing the difference between Two Population Proportions

- Intuitively, we would reject $H_0$ if $\bar{p}_1 - \bar{p}_2$ is sufficiently large.
- To derive a test statistic for the hypothesis tests, we need the sampling distribution of $\hat{p}_1 - \hat{p}_2$.
- As in the 1-sample tests, we shall use normal approximation.
- Step 3: If $H_0$ is true and $p_1 = p_2 = p$, then

$$\frac{\hat{p}_1 - \hat{p}_2}{\sqrt{p(1-p)\left(\frac{1}{n_1}+\frac{1}{n_2}\right)}} \sim N(0,1)$$

where $p$ can be estimated by the pooled estimation

$$\bar{p} = \frac{n_1\bar{p}_1 + n_2\bar{p}_2}{n_1 + n_2}$$

- Step 1: Hypothesis

$H_0: p_1 = p_2$, versus $H_a: p_1 \neq p_2.$

- Step 2: Choose level of significance $\alpha$

- Step 3: Test Statistic

$z = \frac{\bar{p}_1 - \bar{p}_2}{\sqrt{\bar{p}(1 - \bar{p})\left(\frac{1}{n_1} + \frac{1}{n_2}\right)}}$

- Step 4: Rejection Rule

reject $H_0$, if $|z_{obs}| \ge z_{\alpha/2}$ or p-value $\le \alpha$

## Summary of Parametric Tests for Population Means

| Test | What It Does | Test Statistic |
|---|---|---|
| 1-sample Z test | Compare 1 mean with a specified value (variance is known) | $z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}}$ |
| 1-sample T test | Compare 1 mean with a specified value (variance is unknown) | $t = \frac{\bar{x} - \mu_0}{s / \sqrt{n}}$, d.f. = n - 1 |
| 2-sample Z test | Compare 2 independent means (variances are known) | $z = \frac{\bar{x}_1 - \bar{x}_2 - D_0}{\sqrt{\sigma_1^2 / n_1 + \sigma_2^2 / n_2}}$ |
| 2-sample T test (equal variances) | Compare 2 independent means (variances are equal but unknown) | $t = \frac{\bar{x}_1 - \bar{x}_2 - D_0}{\sqrt{\left(1/n_1 + 1/n_2\right) s_p^2}}$, d.f. = $n_1 + n_2 - 2$ |
| 2-sample T test (unequal variances) | Compare 2 independent means (variances are unequal and unknown) | $t = \frac{\bar{x}_1 - \bar{x}_2 - D_0}{\sqrt{s_1^2 / n_1 + s_2^2 / n_2}}$, d.f. = $\frac{\left(s_1^2 / n_1 + s_2^2 / n_2\right)^2}{\left(s_1^2 / n_1\right)^2 / (n_1 -1) + \left(s_2^2 / n_2\right)^2 / (n_2 -1)}$ |
| 2-sample T test (matched samples) | Compare 2 paired means | $t = \frac{\bar{d}}{s_d / \sqrt{n}}$, d.f. = n - 1 |
| 1-way ANOVA | Compare 3 or more means | Covered later! |

## BUSS2302: Business Statistics
Lecture 5: More Topics in Hypothesis Testing

Instructor : SONG Tingting (宋婷婷)

Antai College of Economics and Management

Shanghai Jiao Tong University

Reference: Ch 11&12 in Statistics for Business & Economics, 13e by Anderson, Sweeney, Williams, Camm, and Cochran (2018)

## Hypothesis Testing

A hypothesis is a statement about the population.

- Population mean μ (one population, two populations) ✓
- How about more three or more populations?
- Population variance σ²
- Population distribution (normality, independence)

Hypothesis testing is a procedure based on sample evidence and probability theory to determine whether the hypothesis is a reasonable statement.

Basic steps of hypothesis testing

- 1. State the hypotheses.
- 2. Select a level of significance.
- 3. Identify the test statistic.
- 4. Formulate a decision rule.
- 5. Take a sample, and arrive at decision.

- 1. Inferences About Population Variances
  - Point Estimation and Interval Estimation
  - Testing for One Population Variance
  - Testing for Two Population Variances

- 2. Testing for Three or More Proportions
  - Multiple Comparison Procedure
  - Chi Square Test

- 3. Testing for Independence

- 4. Testing for Goodness of Fit
  - Multinomial Distribution
  - Normal Distribution

## Inferences About Population Variances

- Point Estimation and Interval Estimation
- Testing for One Population Variance
- Testing for Two Population Variances

Testing for Three or More Proportions

Testing for Independence

Testing for Goodness of Fit
