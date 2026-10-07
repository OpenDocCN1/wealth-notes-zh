# 表2.5 多个样本

| | 样本一 | 样本二 | 样本三 | 样本四 |
|---|---|---|---|---|
| $u_t$ | 0 | -2 | 0 | 3 |
|  | -1 | 2 | 5 | -2 |
|  | 3 | 2 | -1 | 1 |
|  | 3 | -3 | 1 | 2 |
|  | -3 | -3 | -2 | 4 |
| $E_T[u_t]$ | 0.4 | -0.8 | 0.6 | 1.6 |

既然样本均值本身也是一个随机变量，那么一个很自然的问题就是样本均值在不同的样本中是如何变化的，即求解样本均值的方差。从方差的定义出发有：

$$\mathrm{var}(ar{u}) = \mathrm{var}\left(\frac{1}{T}\sum_{t=1}^{T}u_tight) = \frac{1}{T^2}\mathrm{var}\left(\sum_{t=1}^{T}u_tight) \tag{2.100}$$

式中$T$为样本量。在最简单的情形中，假设$u_t$序列满足独立同分布，则式（2.100）可以简化成：

$$\mathrm{var}(ar{u}) = \frac{1}{T^2}\mathrm{var}\left(\sum_{t=1}^{T}u_tight) = \frac{\sigma^2(u_t)}{T} \tag{2.101}$$

将式（2.101）两边开方就得到样本均值的标准误：

$$\mathrm{s.e.}(ar{u}) = \frac{\sigma(u_t)}{\sqrt{T}} \tag{2.102}$$

式（2.102）的结果大概是各位读者在统计课中学到的印象最深的一个式子——当$u_t$满足独立同分布时，样本均值的标准误。对于金融数据（比如收益率）来说，$u_t$序列有非零的自相关性（即$\mathrm{cov}(u_t, u_{t-j}) 
eq 0$），难以满足独立同分布，因此需要得到更一般情况下样本均值$ar{u}$的方差：

$$\mathrm{var}(ar{u}) = \frac{1}{T}\left[ \mathrm{var}(u_t) + \sum_{j=1}^{T}\frac{T-j}{T}\left(\mathrm{cov}(u_t,u_{t-j}) + \mathrm{cov}(u_t,u_{t+j})ight) ight] \tag{2.103}$$

当$T$趋于无穷大时，$(T-j)/T$趋于1，可以求出$\mathrm{var}(ar{u})$的渐进形式：

$$\mathrm{var}(ar{u}) ightarrow \frac{1}{T}\sum_{j=-\infty}^{\infty}\mathrm{var}(u_t,u_{t-j}) \tag{2.104}$$

下面再假设一个特殊的情况，即随机变量$u_t$的总体均值$E[u_t]=0$，并利用方差运算的性质$\mathrm{var}(X,Y)=E[XY]-E[X]E[Y]$可得：