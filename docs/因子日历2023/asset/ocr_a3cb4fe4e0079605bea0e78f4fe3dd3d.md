# 03 7 月 Jul.

星期一
五月十六
宜 质量

# 质量因子

质量因子

通过Gordon成长模型分解为3个维度；计算各维度细分指标的横截面排序并进行z-score标准化，然后加总各指标z-score并进行标准化，得到各维度的得分；加总3个维度的得分并再次标准化，得到各股票的质量因子。

```
Price / BookValue = (Dividend / BookValue) / (RequiredReturn - Growth)
= ((Profit / BookValue) * (Dividend / Profit)) / (RequiredReturn - Growth)
= (Profitability - PayoutRatio) / (RequiredReturn - Growth)
```

# 其中

3个维度分别为：

+   ① Profitability表示盈利能力，包括毛利率比总资产（gross profit over assets, GPOA）、ROE、ROA、现金流比总资产（cash flow over assets, CFOA）、毛利率（gross margin）和盈利中现金的占比（即盈利减去应计项目, ACC）这6个指标；
② Growth表示成长能力，用于反映衡量盈利能力指标的变化率；
③ RequiredReturn表示公司的稳定性或者安全性，包括低beta（BAB）、低杠杆率、低破产概率（O-score）和低ROE波动率；
④ PayoutRatio表示股东所得红利在总利润中的占比，主要用来衡量公司的偿付能力（作者在2019发布的质量因子未使用该维度）。

# 说明

质量因子在衰退期和波动率突然大涨的时期表现最好，在大熊市期间表现也不错，这表明质量因子有助于控制组合的风险。

> 【参考文献】
Asness, Clifford S., Andrea Frazzini, Lasse Heje Pedersen, 2019, Quality Minus Junk, Review of Accounting Studies 24(1), 34-112.

![](bbox=[0.42, 0.91, 0.58, 0.96])