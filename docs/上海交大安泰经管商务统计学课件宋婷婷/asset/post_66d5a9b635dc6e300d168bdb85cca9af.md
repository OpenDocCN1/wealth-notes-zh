
## Determining the Sample Size

- Interval Estimation of μ (100(1 − α)% 置信区间): x̄ ± Δ
- Fix the quantity of Δ: the desired margin of error, E
  - If σ is known, E = zα/2 σ / √n the necessary sample size is:
    n = (zα/2)² σ² / E²
  - If σ is unknown, we can obtain a preliminary or planning value for σ by using:
    - estimate of the population standard deviation computed from data of previous studies
    - sample standard deviation from the preliminary sample
    - judgment or a "best guess" for the value of σ

## Example: Determine the Sample Size

## Example: 统计. 规范

- 在检查表中，我们期望控制误差E = 1元，为确定最终的样本容量，我们要得到σ的初步值
- 通过对某系统样本的样本数据分析，我们发现样本标准差为5.5元，我们从5.5作为σ的初步值
- 查表得z0.025 = 1.96
- 利用前面的公式，我们试算所需样本容量为n = 1.96² × 5.5² / 1 = 116.2. 我们取他117作为最终样本量

## Interval Estimate of Population Proportion p

- The sampling distribution of \(\bar{p}\) \(\xrightarrow{d}\) N\(p, \sqrt{p(1-p)/n}\) : approximated by a normal distribution whenever np ≥ 5 and n(1-p) ≥ 5.

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_125_0.png)

## Interval Estimate of Population Proportion *p*

**Example:** 统计. 规范

某校在现有在校1000名学生中，440学生分一餐作为首选。想构造95%的置信区间来估计总体该校学生三餐的比例。

## Interval Estimate of Population Proportion p

## Example: 统计. 规范

- 泡查中我们发现在1000名学生中，440学生分餐作为首选。想构造95%的置信区间来估计总体学生首选三餐的比例。
- n = 1000, p̄ = 440/1000 = 0.44, zα/2 = 1.96. Then, 0.44 ± 1.96√[0.44(1 − 0.44)/1000] = 0.44 ± 0.031

## Interval Estimate of Population Proportion p

## Example: 统计. 规范

- 假设我们想要在99%的置信性保证样本比例与总体比例的误差在0.03范围内，要抽取多少样本？

## Interval Estimate of Population Proportion p

## Example: 统计. 规模

- 假设我们想要在99%的置信性下观察样本比例与总体比例的差异在0.03范围内，要抽取多少样本？
- $z_{\alpha/2}\sqrt{\frac{p^*(1-p^*)}{n}} = 0.03.$
- 在99%置信率下，$z_{0.005}=2.576.$

Recall that $p^* = 0.44$. Then, $n = \frac{(2.576)^2(0.44)(0.56)}{(0.03)^2} = 1817.$

## Take-away

- Formula in Interval Estimation

| | μ | | p |
|---|---|---|---|
| | σ Known | σ Unknown | |
| Interval Estimate | x̄ ± z_{α/2} σ/√n | x̄ ± t_{α/2} s/√n | p̂ ± z_{α/2} √[p̂(1 - p̂)/n] |
| Sample Size | n = (z_{α/2})² σ² / E² | | n = (z_{α/2})² p*(1 - p*) / E² |

- Interpretation of Confidence Interval

## BUSS2302: Business Statistics
Lecture 4: Hypothesis Testing – Inference about
Population Means and Proportions

**Instructor：SONG Tingting（宋婷婷）**
Antai College of Economics and Management
Shanghai Jiao Tong University

**Reference:** Ch 9&10 in *Statistics for Business & Economics*, 13e
by Anderson, Sweeney, Williams, Camm, and Cochran (2018)

## Introduction to Hypothesis Testing

- Motivations
- Overview of Hypothesis Testing

## Hypothesis Tests with One Population

## Hypothesis Tests with Two Populations

## Motivations: The Lady Tasting Tea

- A tea party in Cambridge, England, in the 1920s
- A lady claimed she can tell whether the tea or milk was added first.
- A statistician in attendance decided to test the claim.
- If you were him, how you decide to do?

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_135_0.png)

## Motivations: The Lady Tasting Tea

- The statistician is the Sir Ronald Fisher!
- He decided to design an *experiment*, a *test* of this woman’s claim.
  - Give the lady 8 cups, 4 of each variety in *random order*.
  - Record *#* she got correct. *sample data*
  - Calculate the *probability* for her getting the number she got correct, assuming that she has no ability (*null hypothesis*).
  - If the *probability (p-value)* is sufficiently small, say less than 5%. (*Decision rule*)
  - Effectively acknowledges the ability of the lady! (*Conclusion*)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_136_0.png)

## History of Hypothesis Testing

- Two theories of statistical testing were developed in early 20th century
  - Fisher's "Significance Testing"
  - Neyman-Pearson "Hypothesis Testing"
- Modern hypothesis testing is a hybrid of the two methods.
- For more stories of these great minds, please refer to the reading materials.

- 1. 结合新纪系统"四世同堂"
- 2. *The Lady Tasting Tea: How Statistics Revolutionized Science in the Twentieth Century*

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_137_0.png)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_137_1.png)

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_137_2.png)
