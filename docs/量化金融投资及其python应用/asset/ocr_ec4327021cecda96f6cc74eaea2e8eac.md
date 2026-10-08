Aver age, ARMA模型。其基本思想是把 AR 和 MA 模型结合在一起，使所使用的参数个数很小。

ARMA(p,q) 模型的形式为

```
$$ r _ { t } = \varphi _ { 0 } + \sum _ { i = 1 } ^ { p } \varphi _ { i } r _ { t - i } + a _ { t } + \sum _ { i = 1 } ^ { q } \theta _ { i } a _ { t - i } $$
```

其中，{a_t}为白噪声序列，p 和 q 都是非负整数。AR(p)模型和 MA(q)模型都是 ARMA(p,q)模型的特殊形式。利用向后推移移算子 B，上述模型可写成

```
$$ ( 1 - \varphi _ { 1 } B - \varphi _ { 2 } B ^ { 2 } - \cdots - \varphi _ { p } B ^ { p } ) r _ { t } = \varphi _ { 0 } + ( 1 - \theta _ { 1 } B - \theta _ { 2 } B ^ { 2 } - \cdots - \theta _ { q } B ^ { q } ) a _ { t } $$
```

(后移算子 B 即回到上一时刻。)

再求 r_t 的期望，得到

```
$$ E ( r _ { t } ) = \frac { \varphi _ { 0 } } { 1 - \varphi _ { 1 } - \varphi _ { 2 } - \cdots - \varphi _ { p } } $$
```

和前面 AR 模型一样，因此有着相同的特征方程：

```
$$ 1 - \varphi _ { 1 } x - \varphi _ { 2 } x ^ { 2 } - \cdots - \varphi _ { p } x ^ { p } = 0 $$
```

该方程所有解的倒数称为该模型的特征根，如果所有的特征根的模都小于1，则该 ARMA模型是平稳的。

有一点很关键：ARMA 模型的应用对应应该为平稳序列。下面的步骤都是建立在假设原序列平稳的条件下的。

# 7.4.1 确定 ARMA(p,q) 模型的阶次

1. 用 PACF、ACF 确定模型阶次

通过观察 PACF 和 ACF 截尾可以分别确定 p,q 的值（限定滞后阶数为 50）。

```
```python
fig = plt.figure(figsize=(20,10))
ax1 = fig.add_subplot(211)
fig = sm.graphics.tsap.plot_acf(data, lags=30, ax=ax1)
ax2 = fig.add_subplot(212)
fig = sm.graphics.tsap.plot_pacf(data, lags=30, ax=ax2)
```
```

生成的图形如图 7-11 所示。

可以看出，模型的阶次应该为(27,27)。然而，对这么高的阶次建模，计算量是巨大的。