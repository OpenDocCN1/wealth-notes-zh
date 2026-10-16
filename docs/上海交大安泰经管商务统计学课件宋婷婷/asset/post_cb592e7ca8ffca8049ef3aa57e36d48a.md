
## Testing Population Proportion: Normal Approximation

- Based on the central limit theorem, we have an easier way to get the critical value c is via normal approximation for large n.

- Step 3: Modify the test statistic to z = (p̂ - p₀) / √[p₀(1 - p₀)/n].

- Step 4: Using the Z-test for the upper-tailed hypothesis testing,
  
  reject H₀ : p ≤ p₀, if (p̂ - p₀) / √[p₀(1 - p₀)/n] ≥ zₐ.

## Extensions to Other Hypotheses for Population Proportion

- An overview of the lower tail, upper tail and two-tailed tests is as follows.
- The test statistic is the same, but the rejection regions are not.

|  | Lower-tailed Test | Upper-tailed Test | Two-tailed Test |
|---|---|---|---|
| Hypotheses | H₀: p ≥ p₀<br>Hₐ: p < p₀ | H₀: p ≤ p₀<br>Hₐ: p > p₀ | H₀: p = p₀<br>Hₐ: p ≠ p₀ |
| Test Statistic | z = (p̂ - p₀) / √[p₀(1-p₀)/n] | z = (p̂ - p₀) / √[p₀(1-p₀)/n] | z = (p̂ - p₀) / √[p₀(1-p₀)/n] |
| Rejection rule:<br>Critical value approach | Reject H₀ if z ≤ -zα | Reject H₀ if z ≥ zα | Reject H₀ if |z| ≥ zα/2 |
| Rejection rule:<br>p-value approach | Reject H₀ if p-value ≤ α | Reject H₀ if p-value ≤ α | Reject H₀ if p-value ≤ α |

## Type I and Type II Errors

Errors from hypothesis testing:

- • **Type I Error:** Rejecting the null hypothesis, $H_0$, when it is true. (False positive, 均球性). Well controlled by the level of significance $\alpha$.
  
- • **Type II Error:** Not to reject the null hypothesis when it is false. (False negative, 均或性). Define it as $\beta$.
  
- • **Question:** How to control Type II error?

| Table of error types | Null hypothesis ($H_0$) |
|---|---|
|  | True | False |
| Decision: Reject null hypothesis ($H_0$) | Correct inference (true negative) $(1-\alpha)$ | Type II error (false negative) $\beta$ |
| Decision: Do not reject null hypothesis ($H_0$) | Type I error (false positive) probability $\alpha$ | Correct inference (true positive) $(1-\beta)$ |

## Type II Error

- The quality of hypothesis test is to **minimize** error rates.
  - To reduce the probability of Type I error, make the α value smaller (naive and efficient)!
  - How to decrease the probability of Type II error?

- **Power (原力):** the probability that the test correctly reject the *H₀* when *Hₐ* is true (i.e., 1 − β)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_166_0.png)

## Type I Error and Type II Error

- Ideally, we hope to find a test that controls both the Type I and Type II errors.
- These two types of error rates are traded off against each other.
- For any given sample set, the effort to reduce one type of error generally results in increasing the other type of error.
- The power always depends on three factors:
  - the statistical significant level we selected in the test. α
  - the magnitude of the effect of interest in the population. |μₐ − μ₀|
  - the sample size used to detect the effect. n
- Question: For a given significance test with significance level α, can we control the Type II error probability β?
- Solution: Increase the number of samples you use.

## Example: Lot-Acceptance Sampling

- A supplier has delivered a shipment of batteries. As the quality control manager, you must make the decision that
  - either accept the entire shipment, or
  - return the shipment because of poor quality.
- Battery Standard: the mean useful life is at least 120 hours.
- Sample: 36 batteries will be selected and tested.
- Assume the population standard deviation is known with σ = 12 hours.

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_168_0.png)

## Example: Lot-Acceptance Sampling

The hypotheses are

$H_0: \mu \ge 120 \quad \text{versus} \quad H_a: \mu < 120$

Select the level of significance $\alpha = 0.05$

Since $\sigma$ is known, we use Z-test, and the test statistic

$z = \frac{\bar{x} - \mu_0}{\sigma / \sqrt{n}} = \frac{\bar{x} - 120}{12 / \sqrt{36}} = \frac{\bar{x} - 120}{2}$

Suppose we use critical value approach. Then we will reject $H_0$ if $z \le -z_\alpha$, i.e.,

$\frac{\bar{x} - 120}{2} \le -1.645 \quad \Rightarrow \quad \bar{x} \le 116.71$

Therefore, shipment should be returned if the average useful life of sampled batteries is no larger than 116.71 hours. The probability of returning a qualified shipment (type I error) is no more than 5%.
