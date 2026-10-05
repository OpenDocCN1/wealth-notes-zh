```
from scipy import stats
import statsmodels.api as sm
%matplotlib inline
In [2]: IndexData = pd.read_csv('01pct.csv')
    IndexData = IndexData.set_index(IndexData['tradeDate'])
    IndexData['colseIndexDiff_1'] = IndexData['closeIndex'].diff(1)  # 1阶差分处理
    IndexData['closeIndexDiff_2'] =
IndexData['colseIndexDiff_1'].diff(1) # 2阶差分处理
    IndexData.plot(subplots=True,figsize=(18,12))
```

Out[2]:

![](bbox=[0.12, 0.312, 0.86, 0.654])

上图中第 1 张图为上证综指部分年份的收盘指数，是一个非平穩时间序列；而下面的两张图为平稳时间序列（当然这里没有检验，只是为了让大家看出差异，关于检验序列的平稳性后续会进行讨论），实际上是对第 1 个序列做了差分处理，方差和均值基本平稳，成为了平稳时间序列，我们在后面会谈到这种处理。

下面给出对平稳性的定义。

（1）严平稳：如果对所有的时刻 t，任意正整数 k 和任意 k 个正整数的联合分布 (t₁,t₂……tₖ)，(rₜ₁,rₜ₂……rₜₖ) 与 (rₜ₁₊ₜ,rₜ₂₊ₜ……rₜₖ₊ₜ) 的联合分布相同，则可称时间序列 {rₜ} 是严平稳的。