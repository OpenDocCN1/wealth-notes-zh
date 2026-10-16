

## 用二项分布进行检验

- 10个样本中有7个“+”。假设我们使用显著性水平 α = 0.1，p值是多少？

- 假设H0成立，则 X ~ B(n,0.5)，其中 n = 10

- p值 = 2 × P(X ≥ 7) = 2 × Σ_{k=7}^{n} C(n,k) 0.5^n = 0.3438 > α

- 因此，我们不能拒绝总体中位数为450的原假设。

## 用正态近似进行检验

- 当样本量 $n$ 较大时，我们可以使用正态近似来简化计算。
- 由于 $\mu_X = 0.5n$，$\sigma_X = \sqrt{0.25n}$，则对于较大的 $n$

$$Z := \frac{X - 0.5n}{\sqrt{0.25n}} \sim N(0,1)$$

- 当样本量 n 较大时，我们可以使用正态近似来简化计算。
- 由于 μ_X = 0.5n，σ_X = √0.25n，则对于较大的 n
$$Z := \frac{X - 0.5n}{\sqrt{0.25n}} \sim N(0,1)$$
- 利用正态近似，可以得到拒绝规则。
  - 临界值法：
    若 |Z| ≥ z_{α/2}，则拒绝 H₀。
  - p值法：
    若 p值 = P(|N(0,1)| ≥ |Z|) ≤ α，则拒绝 H₀。

## 符号检验：两个匹配样本的检验

- Sun Coast Farms生产一种名为Citrus Valley的橙汁产品。其主要竞争对手是Tropical Orange。
- 我们想知道消费者对这两个品牌的偏好是否存在差异。
- 14个人分别品尝了两种橙汁产品的无标记样品。

| 个体 | 偏好 | 个体 | 偏好 |
|---|---|---|---|
| 1 | Tropical Orange | 8 | Tropical Orange |
| 2 | Tropical Orange | 9 | Tropical Orange |
| 3 | Citrus Valley | 10 | 无偏好 |
| 4 | Tropical Orange | 11 | Tropical Orange |
| 5 | Tropical Orange | 12 | Citrus Valley |
| 6 | 无偏好 | 13 | Tropical Orange |
| 7 | Tropical Orange | 14 | Tropical Orange |

- 对于每位顾客，
  - 如果更偏好Citrus Valley，则记录加号
  - 如果更偏好Tropical Orange，则记录减号。
  - 如果没有偏好，则不记录（在后续分析中删除）
- 设 $p$ 为偏好Citrus Valley橙汁的顾客比例。这转化为对总体比例的假设检验：

$H_0: p = 0.5,\quad\text{versus}\quad H_a: p \ne 0.5.$

| 个体 | 偏好 | 符号 | 个体 | 偏好 | 符号 |
|---:|---|:---:|---:|---|:---:|
| 1 | Tropical Orange | − | 8 | Tropical Orange | − |
| 2 | Tropical Orange | − | 9 | Tropical Orange | − |
| 3 | Citrus Valley | + | 10 | Tropical Orange | − |
| 4 | Tropical Orange | − | 11 | Citrus Valley | + |
| 5 | Tropical Orange | − | 12 | Citrus Valley | + |
| 6 | 无偏好 |  | 13 | Tropical Orange | − |
| 7 | Tropical Orange | − | 14 | Tropical Orange | − |

## Mann-Whitney-Wilcoxon检验

- 该检验是另一种用于确定两个总体之间是否存在差异的非参数方法。
- 该检验基于两个独立样本。
- 该方法的优点包括：
  - 既可用于有序数据，也可用于定量数据
  - 不需要假设总体服从正态分布
- 该方法不是检验两个总体中位数之间的差异，而是检验两个总体是否相同（两个总体相同）

## 示例

- 银行经理正在监控两个分支机构的账户余额，想知道它们是否相同。
- 假设为：
  - H₀：两个账户余额总体相同
  - Hₐ：两个账户余额总体不相同
- 抽取两个独立的支票账户样本

| 分支机构1 | | 分支机构2 | |
|---|---|---|---|
| 账户 | 余额（美元） | 账户 | 余额（美元） |
| 1 | 2005 | 1 | 850 |
| 2 | 995 | 2 | 950 |
| 3 | 1300 | 3 | 915 |
| 4 | 925 | 4 | 850 |
| 5 | 950 | 5 | 800 |
| 6 | 950 | 6 | 750 |
| 7 | 865 | 7 | 865 |
| 8 | 985 | 8 | 1000 |
| 9 | 875 | 9 | 1050 |
| 10 | 1025 | 10 | 935 |
| 11 | 1025 | | |
| 12 | 975 | | |

