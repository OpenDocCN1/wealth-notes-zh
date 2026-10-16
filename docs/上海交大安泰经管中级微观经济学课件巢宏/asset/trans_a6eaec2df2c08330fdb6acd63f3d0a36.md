

# 布局与可能的投注

![](img/e3f3f402f74cf453caf4596d2d7d7398_1900_0.png)

## 内部投注：

- A  直注 (36 for 1; 35 to 1)
- B  线注 (18 for 1; 17 to 1)
- C  行注 (12 for 1; 11 to 1)
- D  角注 (9 for 1; 8 to 1)
- E  六注 (6 for 1; 5 to 1)

## 外部投注：

- F, G, H  等额投注 (红/黑, 偶/奇, 高/低)
- I          2 to 1 投注 (前12, 中12, 后12)
- 若出现 0 或 00 则全部输掉 (替代规则)

几乎所有投注的庄家优势为 5.26% (5-way 7.89%, 罕见)

## 轮盘统计：外部

![](img/e3f3f402f74cf453caf4596d2d7d7398_1901_0.png)

## 投注统计：外部

![](img/e3f3f402f74cf453caf4596d2d7d7398_1902_0.png)

## GF vs. HO：外部投注

押注对抗 (GF) 或跟随 (HO) 长度为 n 的连胜

使用第三注 (1-12, 13-24, 25-36)

![](img/e3f3f402f74cf453caf4596d2d7d7398_1904_0.png)

## 轮盘统计：内部

**904** 次轮盘旋转

预测结果频率 = 1/38 = **0.026**

![](img/e3f3f402f74cf453caf4596d2d7d7398_1905_0.png)

出现频率最高的结果 = #30 = **0.037**

**注：** 需要约 30,000 次旋转才能得出显著不同于随机的结论 @ p<.10 (Keren & Lewis, 1994)

出现频率最低的结果 = #26 = **0.017**

## 投注统计：内部

22,527 次数字投注

预测投注频率 (若等概率) = 1/38 = 0.026

![](img/e3f3f402f74cf453caf4596d2d7d7398_1906_0.png)

投注频率最高 = #17 = 0.047

投注频率最低 = #15 = 0.015

## 说明性示例

频率- 玩家 130

频率

投注次数- 轮盘顺序

![](img/e3f3f402f74cf453caf4596d2d7d7398_1908_0.png)

## GF vs. HO：内部投注--汇总

### Logit 回归

$P_{ijt}=1$ if person $i$ placed a bet on number $j$ on spin $t$, 0 otherwise

$H_{ijt}$ = how many times has number $j$ appeared previous to spin $t$ but while person $i$ was at the table minus its expected frequency of appearance $(1-(37/38)^{t-1})$

$F_{ijt}$ = favorite numbers, % of spins player $i$ has bet on number $j$ previous to period $t$

$L_{ijt}$ = winning chip left on table

|  | (1) | (2) | (3) | (4) |
|---|---:|---:|---:|---:|
| Intercept | -1.0606** | -1.9989** | -1.0859** | -2.0100** |
| Hotness ($H_{ijt}$) | 0.2987** | 0.4282** | 0.2650** | 0.4049** |
| Favorite ($F_{ijt}$) |  | 32.4900** |  | 32.3100** |
| Leave ($L_{it}$) |  |  | 1.5403** | 0.8800** |
|  |  |  |  |  |
| Chi-squared | 84** | 16776** | 457** | 16879** |
| Log Likelihood | 98687 | 81995 | 98315 | 81892 |
| Pseudo $R^2$ | .01 | .17 | .01 | .17 |

** p < 0.01  * p < 0.05  ^ p < 0.10

## GF vs. HO：内部投注--个人

对每个 139 名赌徒进行相同回归，以热度系数为特征

| Coefficient | 112 possible regressions | 39 regressions w/o errors |
|---|---:|---:|
| Negative Significant | 17 | 9 |
| Negative Nonsignificant | 39 | 11 |
| Positive Nonsignificant | 37 | 10 |
| Positive Significant | 19 | 9 |

一半玩家 GF，一半玩家 HO

**结论：** 汇总证据支持 HO 但存在大量个体异质性

## 调和结果

为何汇总为 HO 但个体层面 GF 和 HO 各占一半？

### 可能性 1：HO 赌徒下注更多

HO 赌徒停留更久 (35.7 次旋转 vs 34.1 次旋转)

...但每次旋转下注较少 (11.0 个数字 vs 14.2 个数字)

### 可能性 2：HO 赌徒在玩法上更一致

55% 的 GF 玩家为不显著，仅 52% 的 HO 玩家为

| 热度系数的平均值 | GF | HO |
|---|---|---|
| 全部 112 | -4.01 | 4.28 |
| 显著 36 | -5.51 | 6.53 |
| 全部 39 | -1.38 | 2.06 |
| 显著 18 | -2.31 | 3.54 |

## HH vs. SL：内部投注--汇总

赢钱时增加投注、输钱时减少投注 (HH) 或赢钱时减少投注、输钱时增加投注 (SL)

### 投注变化的两种衡量

- 停止投注：80% 在输后停止，20% 在赢后停止 (基准 28% 赢)
- 赢后停止的可能性更低，HH

赢/输后平均投注数字

|  | Avg. | Std. | N |
|---|---|---|---|
| 首次旋转 | 7.63 | 6.12 | 139 |
| 上一旋转获胜 | 13.62 | 6.60 | 570 |
| 上一旋转输掉 | 9.21 | 5.35 | 1487 |
| 总体 | 10.26 | 6.09 | 2196 |

t-test, p<.0001, 与汇总的 HH 一致

