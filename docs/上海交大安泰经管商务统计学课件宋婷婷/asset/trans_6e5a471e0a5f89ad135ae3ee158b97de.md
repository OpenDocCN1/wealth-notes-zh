

## 两个总体均值的假设检验

- 一般而言，假设检验有 3 种形式：
  - **双尾检验：**
    \(H_0: \mu_1-\mu_2=D_0,\quad\text{versus}\quad H_a: \mu_1-\mu_2\ne D_0.\)
  - **上尾检验：**
    \(H_0: \mu_1-\mu_2\le D_0,\quad\text{versus}\quad H_a: \mu_1-\mu_2>D_0.\)
  - **下尾检验：**
    \(H_0: \mu_1-\mu_2\ge D_0,\quad\text{versus}\quad H_a: \mu_1-\mu_2<D_0.\)

如果我们想检验两个计算机总体平均年龄是否存在差异，如何建立假设？

### 当方差已知时两个总体均值

考虑双尾假设检验

- 第 1 步：陈述假设

  \(H_0: \mu_1 - \mu_2 = D_0\)   versus   \(H_a: \mu_1 - \mu_2 \ne D_0\).

- 第 3 步：**我们需要一个在 \(H_0\) 下分布已知的检验统计量。**

当 \(\sigma_1^2, \sigma_2^2\) 已知时，检验统计量由下式给出

\[
z = \frac{\bar{x}_1 - \bar{x}_2 - D_0}{\sqrt{\sigma_1^2/n_1 + \sigma_2^2/n_2}}.
\]

第 4 步：在 \(H_0\) 下，我们有 \(z \sim N(0,1)\)。因此，将使用 Z 检验的决策规则。

### 当 \(\sigma_1^2, \sigma_2^2\) 已知时的检验程序

- 检验统计量相同，但拒绝域不同。
- p 值也不同。
  - 双尾检验：p-value = \(P(\lvert z\rvert \ge z_{\text{obs}} \mid \mu_1-\mu_2=D_0)\)
  - 上尾检验：p-value = \(P(z \ge z_{\text{obs}} \mid \mu_1-\mu_2 \le D_0)\)
  - 下尾检验：p-value = \(P(z \le z_{\text{obs}} \mid \mu_1-\mu_2 \ge D_0)\)

|  | 双尾检验 | 上尾检验 | 下尾检验 |
|---|---|---|---|
| **假设** | \(H_0: \mu_1-\mu_2=D_0\)<br>\(H_a: \mu_1-\mu_2\ne D_0\) | \(H_0: \mu_1-\mu_2\le D_0\)<br>\(H_a: \mu_1-\mu_2>D_0\) | \(H_0: \mu_1-\mu_2\ge D_0\)<br>\(H_a: \mu_1-\mu_2<D_0\) |
| **检验统计量** | \(z=\frac{\bar{x}_1-\bar{x}_2-D_0}{\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}}}\) | \(z=\frac{\bar{x}_1-\bar{x}_2-D_0}{\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}}}\) | \(z=\frac{\bar{x}_1-\bar{x}_2-D_0}{\sqrt{\frac{\sigma_1^2}{n_1}+\frac{\sigma_2^2}{n_2}}}\) |
| **拒绝规则：临界值法** | 若 \(\lvert z_{\text{obs}}\rvert\ge z_{\alpha/2}\) 则拒绝 \(H_0\) | 若 \(z_{\text{obs}}\ge z_\alpha\) 则拒绝 \(H_0\) | 若 \(z_{\text{obs}}\le -z_\alpha\) 则拒绝 \(H_0\) |
| **拒绝规则：p 值法** | 若 p-value \(\le\alpha\) 则拒绝 \(H_0\) | 若 p-value \(\le\alpha\) 则拒绝 \(H_0\) | 若 p-value \(\le\alpha\) 则拒绝 \(H_0\) |

### 当 \(\sigma_1^2 = \sigma_2^2\) 未知时两个总体均值

- 实践中，总体的方差 \(\sigma_1^2\) 和 \(\sigma_2^2\) 通常未知。
- 对于点估计，我们仍可使用 \(\bar{x}_1-\bar{x}_2\)。
- 然而，对于区间估计和假设检验，我们需要估计其标准误 \(\sigma_{\bar{x}_1-\bar{x}_2}=\sqrt{\sigma_1^2/n_1+\sigma_2^2/n_2}\)。
- 基本思路：用样本方差估计总体方差，并使用 t 分布代替正态分布。
  - 当 \(\sigma_1^2=\sigma_2^2\) 时可行
  - 当 \(\sigma_1^2\ne\sigma_2^2\) 时近似可行

### 当 \(\sigma_1^2=\sigma_2^2\) 未知时的检验程序

- 使用 \(s_1^2\) 和 \(s_2^2\) 的加权平均，称为 **合并估计量**，

\[
s_p^2=\frac{n_1-1}{n_1+n_2-2}s_1^2+\frac{n_2-1}{n_1+n_2-2}s_2^2
=\frac{\sum_{i=1}^{2}(n_i-1)s_i^2}{\sum_{i=1}^{2}(n_i-1)}.
\]

- t 检验统计量随后服从自由度为 \((n_1+n_2-2)\) 的 t 分布

\[
t=\frac{\bar{x}_1-\bar{x}_2-D_0}{\sqrt{s_p^2(1/n_1+1/n_2)}}\sim t_{n_1+n_2-2},\quad\text{under }H_0.
\]

- 检验程序如下。
  - 临界值法：若 \(\lvert t_{\text{obs}}\rvert\ge \lvert t_{\alpha/2}\rvert\) 则拒绝 \(H_0\)。
  - p 值法：若 p-value \(=P(\lvert t\rvert\ge \lvert t_{\text{obs}}\rvert \mid H_0)\le\alpha\) 则拒绝 \(H_0\)。

### 当 \(\sigma_1^2 \ne \sigma_2^2\) 未知时两个总体均值

- 接下来假设总体方差 \(\sigma_1^2 \ne \sigma_2^2\) 未知。
- 第 1 步：考虑双尾假设检验

  \(H_0: \mu_1 - \mu_2 = D_0 \quad \text{versus} \quad H_a: \mu_1 - \mu_2 \ne D_0\).

- 第 3 步：应使用 \(t\) 检验，对应的 \(t\) 检验统计量为

  \[
  t = \frac{\bar{x}_1-\bar{x}_2-D_0}{\sqrt{s_1^2/n_1+s_2^2/n_2}}.
  \]

- 即使在 \(H_0\) 下，\(t\) 的分布也是 **未知的**。为什么？
- 为 \(t\) 统计量构造一个 Student \(t\) 分布近似

  \[
  t \sim t_{df}, \quad \text{under } H_0,
  \]

  其中

  \[
  df = \frac{(s_1^2/n_1+s_2^2/n_2)^2}{\frac{(s_1^2/n_1)^2}{n_1-1}+\frac{(s_2^2/n_2)^2}{n_2-1}}.
  \]

> （阅读材料 Welch–Satterthwaite equation - Wikipedia）

