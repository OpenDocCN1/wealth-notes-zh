
## 7.2 线性时间序列

时间序列 $\{r_t\}$ 如果能写成：

$$r_t = \mu + \sum_{i=0}^{\infty} \psi_i a_{t-i}$$

其中，$\mu$ 为 $r_t$ 的均值，$\psi_0=1$，$\{a_t\}$ 为白噪声序列，则称$\{r_t\}$ 为线性序列，称 $a_t$ 为在 $t$ 时刻的新信息 (innovation) 或扰动 (shock)。

很多时间序列是线性的，即是线性时间序列，相应地有很多线性时间序列模型，例如接下来要介绍的 AR、MA、ARMA 都是线性模型，但并不是所有的金融时间序列都是线性的。

对于弱平稳序列，利用白噪声的性质很容易得到 $r_t$ 的均值和方差：

$$E(r_t) = \mu, \quad \text{Var}(r_t) = \sigma^2 \sum_{i=0}^{\infty} \psi_i^2$$

其中，$\sigma_a^2$ 为 $a_t$ 的方差。因为 $\text{Var}(r_t)$ 一定小于正无穷，所以 $\psi_i^2$ 必须是收敛序列，由此满足当 $i \to \infty$ 时，$\psi_i^2 \to 0$。

即随着 $i$ 的增大，远处的扰动 $a_{t-i}$ 对 $r_t$ 的影响会逐渐消失。

到目前为止介绍了一些关于时间序列的基本知识和概念，如平稳性、相关性、白噪声、线性序列。下面介绍一些线性模型。

## 7.2 自回归模型

在 7.1 节中，计算了上证指数部分数据段的 ACF，看表可知间隔为 1 时自相关系数是显著的。这说明在 $t-1$ 时刻的数据 $r_{t-1}$ 在预测 $t$ 时刻时的 $r_t$ 时可能是有用的。

根据这一点可以建立下面的模型：

$$r_t = \phi_0 + \phi_1 r_{t-1} + a_t$$

其中 $\{a_t\}$ 是白噪声序列，这个模型与简单线性回归模型有相同的形式，也称为一阶自回归 (AR)模型，简称 AR(1) 模型。

从 AR(1) 很容易推广到 AR($p$) 模型：

$$r_t = \phi_0 + \phi_1 r_{t-1} + \dots + \phi_p r_{t-p} + a_t$$

### 7.2.1 AR($p$) 模型的根及平稳性检验

先假定序列是弱平稳的，则有

$$E(r_t) = \mu, \quad \text{Var}(r_t) = \gamma_0, \quad \text{Cov}(r_t, r_{t-j}) = \gamma_j$$

其中 $\mu, \gamma_0$ 是常数。因为 $\{a_t\}$ 是白噪声序列，因此有

$$E(a_t) = 0, \quad \text{Var}(a_t) = \sigma_a^2$$

所以有

$$E(r_t) = \phi_0 + \phi_1 E(r_{t-1}) + \phi_2 E(r_{t-2}) + \dots + \phi_p E(r_{t-p})$$

根据平稳性的性质，又有 $E(r_t) = E(r_{t-1}) = \dots = \mu$，从而有

$$\mu = \phi_0 + \phi_1 \mu + \phi_2 \mu + \dots + \phi_p \mu$$

$$E(r_t) = \mu = \frac{\phi_0}{1 - \phi_1 - \phi_2 - \dots - \phi_p}$$

对于上式，假定分母不为 0，将下面的方程称为特征方程：

$$1 - \phi_1x - \phi_2x - \cdots - \phi_{px} = 0$$

该方程所有解的倒数称为该模型的特征根。如果所有的特征根的模都小于 1，则该 AR($p$) 序列是平稳的。

下面就用该方法检验上证指数日收益率序列的平稳性。通过如下代码得到图 7-3 所示的图形。

```python
temp = np.array(data2)                          # 载入收益率序列
model = sm.tsa.AR(temp)
results_AR = model.fit()
plt.figure(figsize=(10, 4))
plt.plot(temp, 'b', label='CHGPct')
plt.plot(results_AR.fittedvalues, 'r', label='AR model')
plt.legend()
```

![](img/123c8fbcf9c4e19d1d2636062a3e4340_82_0.png)

可以看看模型有多少阶：

```python
print len(results_AR.roots)
17
```

可以看出，自动生成的 AR 模型是 17 阶的。关于阶次的讨论放在 7.2.2 节。下面画出模型的特征根来检验平稳性：

```python
pi, sin, cos = np.pi, np.sin, np.cos
r1 = 1
theta = np.linspace(0, 2*pi, 360)
x1 = r1*cos(theta)
y1 = r1*sin(theta)
plt.figure(figsize=(6, 6))
plt.plot(x1, y1, 'k')                              # 画单位圆
roots = 1/results_AR.roots                         # results_AR.roots 是特征方程的解，特征根应该取倒数
for i in range(len(roots)):
    plt.plot(roots[i].real, roots[i].imag, 'r', markersize=8)   # 画特征根
plt.show()
```

可以看出，所有特征根都在单位圆内，如图 7-4 所示，则序列为平稳的。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_83_0.png)

### 7.2.2 AR($p$) 模型的定阶

一般用两种方法来决定 $p$：

- （1）利用偏自相关函数 (Partial Auto Correlation Function, PACF)。
- （2）利用信息量准则函数。

#### 1. 用偏自相关函数确定 $p$

对于偏自相关函数的介绍，这里不详细展开，只重点介绍一个性质：AR($p$) 序列的样本偏自相关函数是 $p$ 步截尾的。所谓截尾，就是快速收敛应该是快速降到几乎为 0 或者在置信区间以内。

具体看下面的例子，还是以前面的上证指数日收益率序列为例。

```python
fig = plt.figure(figsize=(20, 5))
ax1 = fig.add_subplot(111)
fig = sm.graphics.tsa.plot_pacf(temp, ax=ax1)
```

从图 7-5 中可以看出，按照截尾来看，模型阶次 $p$ 在 110 以上，但是 7.2.1 节调用的自动生成的 AR 模型阶数为 17，当然，在实际应用中很少会用这么高的阶次。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_83_1.png)

#### 2. 用信息量准则确定 $p$

现在有以上这么多可供选择的模型，在选择模型时通常采用 AIC。增加自由参数的数目能提高拟合的优良性，AIC 鼓励数据拟合的优良性同时又能尽量避免出现过拟合 (overfitting) 的情况。所以优先考虑的模型应是 AIC 值最小的那一个。赤池 (Akaike) 信息准则的方法是寻找可以最好地解释数据但自由参数最少的模型。目前在选择模型时常用以下 3 个准则：

- (1) AIC (Akaike Information Criterion, 赤池信息量准则)：
  $AIC = -2 \ln L + 2k$
- (2) BIC (Bayesian Information Criterion, 贝叶斯信息量准则)：
  $BIC = -2 \ln L + k \ln n$
- (3) HQIC (Hannan-Quinn Information Criterion, 汉南-奎因信息量准则)：
  $HQIC = -2 \ln L + k \ln (\ln n)$

其中，$L$ 为似然函数值，$k$ 为拟合模型中的参数数量，$n$ 为观测数据的个数。

下面看一看在这 3 种准则下确定的 $p$，仍然以上证指数日收益率序列为例。为了减少计算量，只计算前 10 个数据看看效果。

```python
aicList = []
bicList = []
hqicList = []
for i in range(1, 11): # 从 1 阶开始算
    order = (i, 0)    # 这里使用了 ARMA 模型，order 代表模型的 (p, q) 值，令 q 始终为 0，就只考虑了 AR 的情况
    tempModel = sm.tsa.ARMA(temp, order).fit()
    aicList.append(tempModel.aic)
    bicList.append(tempModel.bic)
    hqicList.append(tempModel.hqic)
plt.figure(figsize=(15, 6))
plt.plot(aicList, 'r', label='aic value')
plt.plot(bicList, 'b', label='bic value')
plt.plot(hqicList, 'k', label='hqic value')
plt.legend(loc=0)
```

从图 7-6 中可以看出，3 个准则在第二点均取到最小值，也就是说，$p$ 的最佳取值应该为 1，这里只计算了前 10 个数据，结果未必正确。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_84_0.png)

图 7-6 3 个信息准则的拟合情况

当然，利用上面的方法逐个计算是很耗时间的，实际上，有专门的函数可以直接按照准则计算出合适的阶次，这个函数是针对 ARMA 模型的，后面再讨论。

### 7.2.3 模型的检验

根据下式：

$$r_{t}=\phi_{0}+\phi_{1} r_{t-1}+\cdots+\phi_{p} r_{t-p}+a_{t}$$

如果模型满足充分条件，其残差序列应该是白噪声，7.1.3 节介绍的 Ljung-Box Q 检验（原文误写为“混成检验”，根据上下文及代码 `gstat=True` 和变量 `q` 推断为 Ljung-Box Q 统计量）可以用来检验残差与白噪声的接近程度。

先求出残差序列：

```python
delta = results_AR.fittedvalues - temp[17:]    # 残差
plt.figure(figsize=(10, 6))
# plt.plot(temp[17:], label='original value')
# plt.plot(results_AR.fittedvalues, label='fitted value')
plt.plot(delta, 'r', label='residual error')
plt.legend(loc=0)
```

生成的图形如图 7-7 所示。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_85_0.png)

图 7-7 残差序列

检查它是不是接近白噪声序列，代码如下：

```python
acf, q, p = sm.tsa.acf(delta, nlags=10, gstat=True) ## 计算自相关系数及 p-value
out = np.c_[range(1,11), acf[1:], q, p]
output = pd.DataFrame(out, columns = ['lag', "AC", "Q", "p-value"])
output = output.set_index('lag')
output
```

输出结果：

| lag | AC | Q | p-value |
| :--- | :--- | :--- | :--- |
| 1 | -0.001228 | 0.000554 | 0.981226 |
| 2 | -0.007834 | 0.023140 | 0.988497 |
| 3 | -0.002311 | 0.025110 | 0.998950 |
| 4 | -0.007182 | 0.044199 | 0.999759 |
| 5 | -0.000231 | 0.044219 | 0.999978 |
| 6 | -0.001315 | 0.044862 | 0.999998 |
| 7 | -0.005744 | 0.057176 | 1.000000 |
| 8 | -0.005663 | 0.069178 | 1.000000 |
| 9 | -0.011057 | 0.115060 | 1.000000 |
| 10 | 0.004697 | 0.123362 | 1.000000 |

观察 p-value 可知，该序列可以视为没有相关性，因此可以认为残差序列接近白噪声。

### 7.2.4 拟合优度及预测

#### 1. 拟合优度

使用下面的统计量来衡量拟合优度：

$$R^2 = 1 - \frac{\text{残差的平方和}}{\text{总的平方和}}$$

但是，对于一个给定的数据集，$R^2$ 是用参数个数的非降函数，为了克服该缺点，推荐使用调整后的 $R^2$：

$$R^2_{Adj} = 1 - \frac{\text{残差的方差}}{\text{总的方差}}$$

它的值在 0、1 之间，越接近 1，拟合效果越好。

下面计算之前的上证指数日收益率的 AR 模型的拟合优度：

```python
score = 1 - delta.var()/temp[17:].var()
print score
0.0405166950061
```

可以看出，模型的拟合效果并不好，当然，这并不重要，也许是这个序列并不适合用 AR 模型拟合。

#### 2. 预测

首先把原来的样本分为训练集和测试集，再来看预测效果，还是以之前的数据为例：

```python
train = temp[:-10]
test = temp[-10:]
output = sm.tsa.AR(train).fit()
output.predict()

predicts = output.predict(355, 364, dynamic=True)
print len(predicts)
comp = pd.DataFrame()
comp['original'] = temp[-10:]
comp['predict'] = predicts
comp
```

| | original | predict |
| :--- | :--- | :--- |
| 0 | -0.00223 | 0.000908 |
| 1 | 0.01022 | -0.001473 |
| 2 | 0.00145 | -0.002109 |
| 3 | 0.01278 | -0.002158 |
| 4 | 0.01024 | -0.000216 |
| 5 | 0.02414 | 0.000116 |
| 6 | 0.00241 | 0.001476 |
| 7 | -0.00089 | -0.000119 |
| 8 | 0.00932 | -0.000520 |
| 9 | -0.00739 | -0.000162 |

该模型的预测结果不太好。是不是可以通过其他模型获得更好的结果呢？有关方法将在后面的几节介绍。

## 7.3 移动平均模型及预测

这里我们直接给出移动平均 (Moving Average, MA) 模型 MA(q) 的形式：
$$r_t = c_0 + a_t - \theta_1 a_{t-1} - \cdots - \theta_q a_{t-q}$$
$c_0$ 为一个常数项。这里的 $a_t$ 是 AR 模型 $t$ 时刻的扰动，可以发现，该模型使用了过去 $q$ 个时期的随机干扰或预测误差非线性地表达当前的预测值。

### 7.3.1 MA(q) 模型的性质

- 1. 平稳性

MA(q) 模型总是弱平稳的，因为它们是白噪声序列 (残差序列) 的有限线性组合。因此，根据弱平稳的性质可以得出两个结论：
$$E(r_t) = c_0, \quad \text{Var}(r_t) = (1 + \theta_1^2 + \theta_2^2 + \cdots + \theta_q^2)\sigma_a^2$$

- 2. 自相关函数

对 MA(q) 模型，其自相关函数 ACF 总是 $q$ 步截尾的。因此 MA(q) 序列只与其前 $q$ 个延迟值线性相关，从而它是一个“有限记忆”的模型。
这一点可以用来确定模型的阶次，后面会介绍。

- 3. 可逆性

当满足可逆条件的时候，MA(q) 模型可以改写为 AR(p) 模型。这里不进行推导，只给出 1 阶和 2 阶 MA 模型的可逆性条件。
1 阶：
$$|\theta_1| < 1$$
2 阶：
$$|\theta_2| < 1, \quad \theta_1 + \theta_2 < 1$$

### 7.3.2 MA(q) 模型的阶次判定

通常利用上面介绍的第二条性质“MA(q) 模型的 ACF 函数是 $q$ 步截尾的”来判断模型阶次。下面使用上证指数的日涨跌数据 (2013 年 1 月至 2014 年 8 月) 来进行分析，先取数据，取数和绘图代码如下：

```python
from scipy import stats
import statsmodels.api as sm            # 统计相关的库
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
IndexData = DataAPI.MktIdxGet(indexID = u"", ticker = u"000001",beginDate = u"20130101",
endDate = u"20140801",field =u"tradeDate,closeIndex,CHGFct", pandas = "i")
IndexData = IndexData.set_index(IndexData['tradeDate'])
data = np.array(IndexData['CHGFct'])                 # 上证指数日涨跌
IndexData['CHGFct'].plot(figsize = (15,5))
```

生成的图形如图 7-8 所示。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_88_0.png)

可以看出，序列看上去是弱平稳的。下面画出序列的 ACF：

```python
fig = plt.figure(figsize=(20,5))
ax1 = fig.add_subplot(111)
fig = sm.graphics.tsa.plot_acf(data,ax=ax1)
```

可以发现，图 7-9 所示的 ACF 在 43 处截尾，之后的 ACF 均在置信区间内，由此可以判定该序列的 MA 模型阶次为 43。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_88_1.png)

### 7.3.3 建模和预测

由于 sm.tsa 中没有单独的 MA 模块，因此利用 ARMA 模块，只要将其中 AR 的阶 $p$ 设为 0 即可。
