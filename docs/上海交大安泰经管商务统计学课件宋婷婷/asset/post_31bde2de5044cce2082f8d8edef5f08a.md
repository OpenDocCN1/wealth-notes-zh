
## Exercises

- Currently a car attains a fuel efficiency of 10 km/l. The research group wants to test if a new fuel injection system provides higher km/l rating than the current system.
    - (a) \(H_0: \mu \ge 10\), versus \(H_a: \mu < 10\)
    - (b) \(H_0: \mu \le 10\), versus \(H_a: \mu > 10\)
    - (c) \(H_0: \mu = 10\), versus \(H_a: \mu \ne 10\)

- The label on a soft drink bottle states it contains 330 ml. A government agency wants to check if the label is incorrect and the bottles are being under-filled.
    - (a) \(H_0: \mu \ge 330\), versus \(H_a: \mu < 330\)
    - (b) \(H_0: \mu \le 330\), versus \(H_a: \mu > 330\)
    - (c) \(H_0: \mu = 330\), versus \(H_a: \mu \ne 330\)

What if it is the manufacturer who wants to know if the bottle-filling process is operating correctly?

## Testing Population Mean when Variance is Known

- Assume we collect \(n\) i.i.d. samples \(X_1, X_2, \ldots, X_n\) and \(X_i \sim N(\mu, \sigma^2)\).

- **Step 1:** State the null and alternative hypotheses \(H_0: \mu = \mu_0;\ H_a: \mu \ne \mu_0\).

- **Step 2:** Select the level of significance \(\alpha\).

- **Step 3:** When the variance is known, the test statistic is specified by

  \[
  z = \frac{\bar{x} - \mu_0}{\sigma/\sqrt{n}}
  \]

  where \(\bar{x}\) is the sample mean.

  - The test procedure based on the \(z\) statistic is often called **Z-test**.
  - Under \(H_0\), the distribution of \(z\) is known: \(z \sim N(0,1)\).

- **Step 4:** Two approaches to determine the rejection rule:
  - Critical-value approach
  - p-value approach

### Critical-Value Approach

- We first specify a critical value for the test statistic.
- The critical value serves as a benchmark for determining whether the value of the test statistic is small (or large) enough to reject.
- Given the critical value \(c\), the rejection region is \((-\infty, -c] \cup [c, \infty)\).
- How to determine \(c\) to achieve a significance level of \(\alpha\)?

  Significance level = \(P(\text{Reject } H_0 \mid H_0) = P(|z| \ge c \mid H_0) = 2\Phi(-c) = 2\bar{\Phi}(c)\)

- The significance level \(\alpha\) is achieved by letting \(c = z_{\alpha/2} = \Phi^{-1}(1 - \alpha/2)\).

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_149_0.png)

- **Step 5:** Comparing the test statistic to a critical value, we make a decision to either reject \(H_0\) or not to reject it.

### p-Value Approach

- P-value provides additional information: the strength of the rejection or non-rejection.
- p-value: the probability of observing a sample value as extreme as, or more extreme than, the value observed, given that \(H_0\) is true.

  p-value = \(P(|z| \ge |z_{\mathrm{obs}}| \mid \mu = \mu_0)\)

- In the statistical test, the p-value is used as a measure of the evidence against \(H_0\) provided by the sample.
- Smaller p-values indicate more evidence against the null hypothesis.
- **Step 5:** The rejection rule using the p-value approach is to reject \(H_0: \mu = \mu_0\) if p-value ≤ \(\alpha\).

### Extensions to Other Hypotheses

- The test procedures are similar for the upper-tailed and lower-tailed hypotheses.
- **Step 1:** Consider the upper-tailed hypothesis \(H_0: \mu \le \mu_0 \quad \text{versus} \quad H_a: \mu > \mu_0\).
- **Step 2:** Select the level of significance \(\alpha\).
- **Step 3:** Using the same test statistic \(z\).
- **Steps 4 & 5:** We reject \(H_0\) if \(z \ge z_\alpha\), where the critical value \(z_\alpha\) is chosen such that the significance level is \(\alpha\).
- **P-value approach:** p-value = \(P(z \ge z_{\mathrm{obs}} \mid H_0)\).

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_151_0.png)

### When Population is not Normally Distributed

- If the population has a normal distribution, both Z-test and T-test are applicable even with small sample sizes (T-test is preferred).
- If the population is not normally distributed, larger sample sizes are required.
  - Advice from the textbook: \(n \ge 30\).
- If the population is not normally distributed, and the sample size is small, we may use nonparametric methods.

### Relationship with Interval Estimation

- For a normal population with \(\sigma\) known, the \((1 - \alpha)\%\) confidence interval for \(\mu\) is given by

  \[
  \bar{x} \pm z_{\alpha/2}\frac{\sigma}{\sqrt{n}}
  \]

- We know that \((1 - \alpha)\%\) of the confidence intervals generated will contain the population mean.
- Therefore, we can also use the confidence interval to conduct a two-tailed test \(H_0: \mu = \mu_0\) versus \(H_1: \mu \ne \mu_0\).
  - Select a simple random sample and develop the \((1 - \alpha)\%\) confidence interval.
  - If it contains the hypothesized value \(\mu_0\), do not reject \(H_0\); otherwise, reject \(H_0\).
