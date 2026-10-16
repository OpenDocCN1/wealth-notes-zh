

## Motivations: Effluent Monitoring (污水监测)

- **Real problems:** 政府希望确保工厂排放的污水（处理过的污水）符合一定的标准。
  - 除了平均 pH 值外，报告的 pH 值的方差也被审查。
- **基于样本 pH 值，我们想知道：**
  - pH 值的（估计）方差是多少？
    - ⇒ 点估计量
  - 方差的 95% 置信区间是多少？
    - ⇒ 区间估计量
  - 该工厂是否具有正常运行工厂的方差？
    - ⇒ 假设检验

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_201_0.png)

## Interval Estimation of Variance and Standard Deviation

- 用 \(\chi^2_{(1-\alpha)}\) 表示卡方分布的 \(\alpha\)-分位数

- 那么有 \((1-\alpha)\) 的概率使得

\[
\chi^2_{(1-\alpha/2)} \leq \frac{(n-1)s^2}{\sigma^2} \leq \chi^2_{\alpha/2}
\]

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_203_0.png)

- 进行代数运算，我们得到方差的 **区间估计量**

\[
\frac{(n-1)s^2}{\chi^2_{\alpha/2}} \leq \sigma^2 \leq \frac{(n-1)s^2}{\chi^2_{(1-\alpha/2)}}
\]

- 开平方，我们得到标准差的 **区间估计量**

\[
\sqrt{\frac{(n-1)s^2}{\chi^2_{\alpha/2}}} \leq \sigma \leq \sqrt{\frac{(n-1)s^2}{\chi^2_{(1-\alpha/2)}}}
\]

## Example——蜜雪冰城

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_205_0.png)

- 背景：如果要论2021登上半壁江山的饮品，蜜雪冰城主题曲绝对算得上其中之一。
- 于1997年创立的蜜雪冰城大概也没有想到，卖了20多年茶，后来却是凭借在网络上被广泛传播的旋律而彻底火了。
- 由于在主题曲大火后不久，蜜雪冰城却被曝出食品安全问题，蜜雪冰城官方也为此作出整改并道歉。

- Consider the production process of filling Ice Lemon Tea in mi xue bing cheng (蜜雪冰城).
- **Real problem:** 我们有兴趣估计蜜雪冰城冰柠茶灌装过程的总体方差。
- **Statistically:** 抽取 20 个容器的样本，灌装量的样本方差为 $s^2 = 0.0025$。
- 我们的兴趣在于以 $\alpha = 0.05$ 建立置信区间。
- 由于 $\chi^2_{0.975}(19) = 8.907$ 且 $\chi^2_{0.025}(19) = 32.852$，置信区间为

$$
\frac{(19)(0.0025)}{32.852} \le \sigma^2 \le \frac{(19)(0.0025)}{8.907},
$$

即，

$$
0.0014 \le \sigma^2 \le 0.0053
$$

$$
0.0380 \le \sigma \le 0.0730
$$

## Hypothesis Testing About a Population Variance

- 关于总体方差的假设检验有三种形式：
  - 双尾检验：$H_0: \sigma^2 = \sigma_0^2$， versus $H_a: \sigma^2 \neq \sigma_0^2$
  - 上尾检验：$H_0: \sigma^2 \leq \sigma_0^2$， versus $H_a: \sigma^2 > \sigma_0^2$
  - 下尾检验：$H_0: \sigma^2 \geq \sigma_0^2$， versus $H_a: \sigma^2 < \sigma_0^2$

- Step 1: state the null and alternative hypotheses
  - $H_0: \sigma^2 \le \sigma_0^2$；$H_a: \sigma^2 > \sigma_0^2$。
- Step 2: select the level of significance $\alpha$.
- Step 3: the test statistic is specified by

$$
\chi^2 = \frac{(n-1)s^2}{\sigma_0^2}.
$$

When $H_0$ is true with equality holds, it follows $\chi^2(n-1)$.

- Step 4: Two approaches to determine the rejection rule
  - Critical-value approach
  - p-value approach

