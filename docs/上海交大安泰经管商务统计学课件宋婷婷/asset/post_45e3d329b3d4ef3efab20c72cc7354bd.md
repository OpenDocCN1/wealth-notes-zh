
## 餐功故事：扩大证行共鸣力

做果云

4. 变体：做果云运练出了两咱皇政迎似：

- 现制寄引
  - e.g. 下发到到多少师，叫他做拿多少师！
  - 认量包包常引为直，机和现案多师常引用排需要分享异
- 等案用标
  - e.g. 认区时直街构为均，减案的同时给治护守少师
  - 皇政区练行大，通达到其用排共需用排主推分享异

5. 实验样本二进行时直：做果云通过A/B之试校南分出小流星进行两咱迎似复构快选试则

6. 分析结果：梁果发现用95%构信险下，参案用样皇政构主推条与底全论数据正向运36%，主推分享论数据正向运升117%异

7. 从数据到决策：如果果云做果运管主科，订构选择术？

## An Academic Example

- Breza E, Stanford FC, Aisan M, et al. [Effects of a large-scale social media advertising campaign on holiday travel and COVID-19 infections: a cluster randomized controlled trial](https://doi.org/10.1038/s41586-021-04150-4). *Nature Medicine*, 2021, 27(9): 1622–1628.
- [Some facts](#): During the COVID-19 crisis, many healthcare professionals used social media to spread public health messages.
- [Question](#): But will these messages really influence people’s behavior at scale?

## An Academic Example

But will these messages really influence people's behavior at scale?

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_400_0.png)

## An Academic Example

- **Experiments:** To figure it out, some nurses and doctors had recorded some videos to encourage people staying at home for the coming Thanksgiving holiday (26, Nov) and then disseminated to Facebook users as ads, see: Covid19Video
- It’s known that holiday travel would lead to a surge in the epidemic
- Now we want to design an experiment to see whether these short videos would cause a decline in COVID-19 cases after the holidays

## Design of experiment

OEC (Response Variable)

- **Q1:** How to choose a proper OEC?
  - Basic principles:
    - using **short-term metrics** that predict **long-term value** (and hard to game);
    - can be practically evaluated;
    - reliable data can be collected, ideally cheap

## Design of experiment

OEC (Response Variable)

- Q1: How to choose a proper OEC?
  - Basic principles:
    - using short-term metrics that predict long-term value (and hard to game);
    - can be practically evaluated;
    - reliable data can be collected, ideally cheap
  - We may choose zip code-level COVID-19 infections reported to state health authorities as OEC

- Q2: What is the randomization unit?
  - The randomization unit will be Facebook users in experimental areas

## Design of Experiment

Randomization Units

- Q3: What population of randomization units do we want to target? How should we randomize them?

## Design of Experiment

Randomization Units

- Q3: What population of randomization units do we want to target? How should we randomize them?
  - The researchers had selected 13 states where the data needed are available, then:
    - Randomly allocated counties to be 'high-intensity (H)' or 'low-intensity' (L) with probability 1/2 each
    - In H/L counties, randomized zip codes into intervention with probability 3/4 / 1/4 and control with probability 1/4 / 3/4
    - The videos were only sent to the users in intervention areas
    - 随机化单元：县级次数；建议或并
    - 实验组有两个次级；高随机二低级处理！

## Design of Experiment

Randomization Units

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_407_0.png)

Treated
Control
Not in Study

## Design of Experiment

Sample Size

- Q4: How large (size) does our experiment need to be?
  - The dataset, a sample of 4925 zip codes can provide 80% power and 5% significance to detect standardized difference 0.057.
  - In the paper, they have 6998 zip-codes.
  - Recall Lecture 4:
    - Type I error: α; Type II error: β; Power: 1 − β
    - μ0 and μ1: Means under the null and alternative hypotheses
    - n0 and n1: sample sizes in two groups (control and treatment) (may be the same)
  - Given the above information, what is the desirable sample size?

## Design of Experiment

Sample Size

![](img/ea9cdfad8dc7821ead4e68fc7aaa5ca3_409_0.png)

Fig. 2.7 Sampling model for two independent sample case. Two-tailed alternatives, equal variances under null and alternative hypotheses.

Two-tailed Tests: n = [2 (Z_{1 - α/2} + Z_{1 - β})² σ²] / (μ0 - μ1)²
