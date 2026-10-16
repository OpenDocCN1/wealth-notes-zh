# Understand $t = \frac{\bar{x} - \mu}{s/\sqrt{n}}$

We decompose $t = \frac{\bar{x} - \mu}{s/\sqrt{n}}$ as follows

$t = \frac{\frac{\bar{x} - \mu}{\sqrt{\sigma^2/n}}}{\sqrt{\frac{s^2 (n-1)/\sigma^2}{n-1}}} = \frac{N(0,1)}{\sqrt{\chi^2 (n-1)/n-1}}$

+   - Assumption: Let each element $x_1, x_2, ..., x_n$ be $N(\mu, \sigma^2)$ r.v.

+   - Numerator: $\frac{\bar{x} - \mu}{\sqrt{\sigma/n}} \sim N(0,1)$

+   - Denominator: $\frac{(n-1)s^2}{\sigma^2} \sim \chi^2(n-1)$

+   - $\bar{x}$ and $s$ are independent (Why?)