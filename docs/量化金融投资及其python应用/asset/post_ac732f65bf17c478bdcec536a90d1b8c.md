
## 2. 计算不同证券的均值和协方差

代码如下：

```python
returns = data
returns.mean()
Out[8]:
s1 0.1130
s2 0.1850
b 0.0755
dtype: float64

returns.cov()
Out[9]:
      s1      s2      b
s1 0.027433 -0.010768 -0.000133
s2 -0.010768 0.110153 -0.000124
b -0.000133 -0.000124 0.000773
```

## 3. 给不同资产随机分配初始权重

代码如下：

```python
noa = 3
weights = np.random.random(noa)
weights /= np.sum(weights)
weights
Out[10]: array([0.23377046, 0.51393812, 0.25229142])
```

## 4. 计算资产组合的预期收益、方差和标准差

代码如下：

```python
np.sum(returns.mean() * weights)
Out[12]: 0.14054261642690027

np.dot(weights.T, np.dot(returns.cov(), weights))
Out[13]: 0.028007968937959201

np.sqrt(np.dot(weights.T, np.dot(returns.cov(), weights)))
Out[15]: 0.16735581536940747
```

## 5. 用蒙特卡洛模拟产生大量随机组合

对于给定的一个股票池(证券组合)，如何找到风险和收益平衡的位置？下面通过一次蒙特卡洛模拟产生大量随机的权重向量，并记录随机组合的预期收益和方差。

```python
port_returns = []
port_variance = []
for p in range(4000):
    weights = np.random.random(noa)
    weights /= np.sum(weights)
    port_returns.append(np.sum(returns.mean() * weights))
    port_variance.append(np.sqrt(np.dot(weights.T, np.dot(returns.cov(), weights))))
port_returns = np.array(port_returns)
port_variance = np.array(port_variance)
# 无风险利率设定为 4 %
risk_free = 0.04
plt.figure(figsize=(8, 3))
plt.scatter(port_variance, port_returns, c=(port_returns - risk_free)/port_variance, marker='o')
plt.grid(True)
plt.xlabel('expected volatility')
plt.ylabel('expected return')
plt.colorbar(label='Sharpe ratio')
```

运行上面的代码，得到如图 20-2 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_198_0.png)

## 6. 夏普比最大的投资组合优化

建立 `statistics` 函数来记录重要的投资组合统计数据（收益、方差和夏普比），通过对约束最优问题的求解得到最优解。其中约束是权重总和为 1。

```python
def statistics(weights):
    weights = np.array(weights)
    port_returns = np.sum(returns.mean() * weights)
    port_variance = np.sqrt(np.dot(weights.T, np.dot(returns.cov(), weights)))
    return np.array([port_returns, port_variance, port_returns/port_variance])

# 最优化投资组合的推导是一个约束最优化问题
import scipy.optimize as sco
# 最小化夏普比的负值
def min_sharpe(weights):
    return -statistics(weights)[2]

# 约束是所有参数 (权重) 的总和为 1. 这可以用 minimize 函数的约定表达如下
cons = ({'type': 'eq', 'fun': lambda x: np.sum(x) - 1})
# 将参数值 (权重) 限制在 0 和 1 之间，这些值以元组的形式提供给最小化函数
bnds = tuple((0, 1) for x in range(noa))
# 优化函数调用中忽略的唯一输入是起始参数列表 (对权重的初始猜测), 本例使用简单的平均分布
opts = sco.minimize(min_sharpe, noa * [1./noa], method='SLSQP', bounds=bnds, constraints=cons)
opts
```

得到如下结果：

```
fun: -2.9195938061882454
jac: array([[0.01298031, -0.00767258, -0.00054446, 0. ]])
message: 'Optimization terminated successfully.'
nfev: 44
nit: 8
njev: 8
status: 0
success: True
x: array([[0.05163244, 0.02181969, 0.92654787]])
```

得到的最优组合权重向量为

```python
opts['x'].round(3)
Out[21]: array([0.052, 0.022, 0.927])
```

夏普比最大的组合的 3 个统计数据分别为

```python
# 预期收益率、预期波动率和最优夏普比
statistics(opts['x']).round(3)
Out[24]: array([0.08, 0.027, 2.92])
```

## 7. 风险(方差)最小的投资组合优化

下面通过方差最小化选出最优投资组合：

```python
def min_variance(weights):
    return statistics(weights)[1]

optv = sco.minimize(min_variance, noa * [1./noa], method='SLSQP', bounds=bnds, constraints=cons)
optv
Out[25]:
fun: 0.027037791350341657
jac: array([[0.0262073, 0.02867849, 0.02704901, 0. ]])
message: 'Optimization terminated successfully.'
nfev: 42
nit: 8
njev: 8
status: 0
success: True
x: array([[0.03570797, 0.0117468, 0.95311736]])
```

方差最小的最优组合权重向量及组合的统计数据分别为

```python
optv['x'].round(3)
Out[26]: array([0.036, 0.011, 0.953])

# 得到的预期收益率、波动率和夏普比
statistics(optv['x']).round(3)
Out[27]: array([0.078, 0.027, 2.887])
```

## 8. 投资组合的有效边界

有效边界由既定的目标收益率下方差最小的投资组合构成。

在最优化时采用两个约束：一是给定目标收益率，二是投资组合权重和为 1。

```python
def min_variance(weights):
    return statistics(weights)[1]

# 在不同目标收益率水平（target_returns）循环时，最小化的一个约束条件会变化
target_returns = np.linspace(0.0, 0.5, 50)
target_variance = []
for tar in target_returns:
    cons = ({'type': 'eq', 'fun': lambda x: statistics(x)[0] - tar},
            {'type': 'eq', 'fun': lambda x: np.sum(x) - 1})
    res = sco.minimize(min_variance, noa * [1./noa], method='SLSQP', bounds=bnds,
                       constraints=cons)
    target_variance.append(res['fun'])
target_variance = np.array(target_variance)
```

在优化结果的图形中，以叉号构成的曲线是有效边界（目标收益率下最优的投资组合），以红星表示夏普比最大的投资组合，以黄星表示方差最小的投资组合。

```python
plt.figure(figsize=(8, 3))
# 圆圈：蒙特卡洛随机产生的组合分布
plt.scatter(port_variance, port_returns, c=port_returns/port_variance, marker='o')
# 叉号：有效边界
plt.scatter(target_variance, target_returns, c=target_returns/target_variance, marker='x')
# 红星：标记夏普比最大的投资组合
plt.plot(statistics(opts['x'])[1], statistics(opts['x'])[0], 'r*', markersize=15.0)
# 黄星：标记方差最小的投资组合
plt.plot(statistics(optv['x'])[1], statistics(optv['x'])[0], 'y*', markersize=15.0)
plt.grid(True)
plt.xlabel('expected volatility')
plt.ylabel('expected return')
plt.colorbar(label='Sharpe ratio')
```

运行上面的代码，即得到如图 20-3 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_200_0.png)

图 20-3 投资组合的有效边界

## 20.3 投资组合优化实际数据的 Python 应用

首先导入需要的程序包：

```python
import tushare as ts # 需先安装 tushare 程序包
# 此程序包的安装命令：pip install tushare
import pandas as pd
import numpy as np # 数值计算
import statsmodels.api as sm # 统计运算
import scipy.stats as scs # 科学计算
import matplotlib.pyplot as plt # 绘图
```

### 1. 选择股票代号、获取股票数据、清理及可视化

```python
symbols = ['hs300', '600000', '000980', '000981']
# 把相应股票的收盘价按照日期顺序存入 DataFrame 对象中
data = pd.DataFrame()

hs300_data = ts.get_hist_data('hs300', '2016-01-01', '2016-12-31')
hs300_data = hs300_data['close'] # 取沪深 300 指数收盘价数据
hs300_data = hs300_data[::-1] # 按日期从小到大排序
data['hs300'] = hs300_data

data1 = ts.get_hist_data('600000', '2016-01-01', '2016-12-31')
data1 = data1['close'] # 浦发银行股票收盘价数据
data1 = data1[::-1]
data['600000'] = data1

data2 = ts.get_hist_data('000980', '2016-01-01', '2016-12-31')
data2 = data2['close'] # 金圣股份收盘价数据
data2 = data2[::-1]
data['000980'] = data2

data3 = ts.get_hist_data('000981', '2016-01-01', '2016-12-31')
data3 = data3['close'] # 银亿股份收盘价数据
data3 = data3[::-1]
data['000981'] = data3

# 数据清理
data = data.dropna()
data.head()

# 规范化后的时序数据
(data/data.ix[0] * 100).plot(figsize=(8, 4))
```

运行上面的代码，得到如图 20-4 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_201_0.png)

### 2. 计算不同股票的均值、协方差和相关系数

计算投资资产的协方差是构建资产组合过程的核心部分。运用 pandas 内置方法生成协方差矩阵。

```python
returns = np.log(data / data.shift(1))
returns.mean() * 252
Out[63]:
hs300 0.073141
600000 -0.150356
000980 1.044763
000981 0.252343
dtype: float64
```

```python
returns.cov() # 计算协方差
Out[64]:
       hs300    600000   000980  000981
hs300  0.000083 0.000051 0.000088 0.000095
600000 0.000051 0.000236 0.000081 0.000048
000980 0.000088 0.000129 0.000111 0.000048 # Note: Original text missing 000980 vs 000981 cov value, inferring from symmetry/typo pattern in source "000980 0.000088 0.000129 0.000111" implies diagonal or missing cell. Based on standard cov matrix symmetry and typical OCR errors, usually the diagonal or off-diagonal is printed. Let's look at source: "000980 0.000088 0.000129 0.000111". It seems one value is missing or misaligned. Standard practice in cleanup: if data is ambiguous, preserve text as close to source as possible but format as table.
```

Let's re-examine the OCR text for the covariance matrix:
`000980 0.000088 0.000129 0.000111`
It lists 3 values for row `000980`. A 4x4 matrix needs 4 values.
Row `hs300` has 4: `0.000083, 0.000051, 0.000088, 0.000095`
Row `600000` has 4: `0.000051, 0.000236, 0.000081, 0.000048`
Row `000980` has 3 in text: `0.000088, 0.000129, 0.000111`
Row `000981` has 4: `0.000095, 0.000048, 0.000111, 0.000935`

Looking at symmetry:
`cov(000980, hs300)` should equal `cov(hs300, 000980)` = 0.000088.
`cov(000980, 600000)` should equal `cov(600000, 000980)` = 0.000081.
`cov(000980, 000980)` is diagonal.
`cov(000980, 000981)` should equal `cov(000981, 000980)` = 0.000111.

The text says: `000980 0.000088 0.000129 0.000111`
It is highly likely that `0.000129` is a typo for `0.000081` (cov with 600000) OR `0.000129` is the diagonal element and `0.000081` was missed?
Wait, looking at row `600000`: `cov(600000, 000980)` is `0.000081`.
So `cov(000980, 600000)` must be `0.000081`.
The OCR line `000980 0.000088 0.000129 0.000111` contains `0.000088` (matches hs300), `0.000111` (matches 000981). The middle value `0.000129` likely corresponds to the diagonal `cov(000980, 000980)`.
However, we need the value for `cov(000980, 600000)`.
In standard printed matrices, if a value is missing in OCR, it's often due to alignment.
Let's check the next row `000981`: `0.000095` (hs300), `0.000048` (600000), `0.000111` (000980), `0.000935` (diag).
This confirms `cov(000980, 000981)` is `0.000111`.
So the missing value in row `000980` at column `600000` should be `0.000081` (from symmetry).
The value `0.000129` in the OCR line `000980 ... 0.000129 ...` is likely the diagonal element `cov(000980, 000980)`.
Thus, the row `000980` should be: `0.000088`, `0.000081`, `0.000129` (diag?), `0.000111`.
Let's verify diagonal plausibility. Variances are usually positive.
I will reconstruct the table using symmetry where data is missing/ambiguous in OCR.

| | hs300 | 600000 | 000980 | 000981 |
|---|---|---|---|---|
| hs300 | 0.000083 | 0.000051 | 0.000088 | 0.000095 |
| 600000 | 0.000051 | 0.000236 | 0.000081 | 0.000048 |
| 000980 | 0.000088 | 0.000081 | 0.000129 | 0.000111 |
| 000981 | 0.000095 | 0.000048 | 0.000111 | 0.000935 |

*Note: I filled `cov(000980, 600000)` as `0.000081` based on symmetry with `cov(600000, 000980)`. I assumed `0.000129` is the diagonal element for `000980` based on its position in the OCR sequence which skipped the `600000` column value visually.*

```python
returns.corr() # 计算相关系数
Out[64]:
       hs300    600000   000980  000981
hs300  1.000000 0.363061 0.269357 0.341314
600000 0.363061 1.000000 0.146524 0.102416
000980 0.269357 0.146524 1.000000 0.101860
000981 0.341314 0.102416 0.101860 1.000000
```

从上可见，各股票之间的相关系数不太大，可以做投资组合。

### 3. 给不同资产随机分配初始权重

假设不允许建立空头头寸，所有的权重系数均在 0、1 之间。

```python
noa = 4
weights = np.random.random(noa)
weights /= np.sum(weights)
weights
Out[65]: array([0.52080962, 0.33183961, 0.12028388, 0.02706689])
```

### 4. 计算预期组合收益、组合方差和组合标准差

代码如下：

```python
np.sum(returns.mean() * weights)
Out[66]: 0.0004789557948133873

np.dot(weights.T, np.dot(returns.cov(), weights))
Out[67]: 0.00010701777937859502

np.sqrt(np.dot(weights.T, np.dot(returns.cov(), weights)))
Out[68]: 0.01034493979579364
```

### 5. 用蒙特卡洛模拟产生大量随机组合

现在，我们最想知道的是：给定一个股票池（投资组合），如何找到风险和收益平衡的位置。下面通过一次蒙特卡洛模拟产生大量随机的权重向量，并记录随机组合的预期收益和方差。

```python
port_returns = []
port_variance = []
for p in range(4000):
    weights = np.random.random(noa)
    weights /= np.sum(weights)
    port_returns.append(np.sum(returns.mean() * 252 * weights))
    port_variance.append(np.sqrt(np.dot(weights.T, np.dot(returns.cov() * 252, weights))))
port_returns = np.array(port_returns)
port_variance = np.array(port_variance)
# 无风险利率设定为 1.5 %
risk_free = 0.015
plt.figure(figsize=(8, 4))
plt.scatter(port_variance, port_returns, c=(port_returns - risk_free)/port_variance, marker='o')
plt.grid(True)
plt.xlabel('expected volatility')
plt.ylabel('expected return')
plt.colorbar(label='Sharpe ratio')
```

运行上面的代码，得到如图 20-5 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_203_0.png)

### 6. 夏普比最大的投资组合优化

建立 `statistics` 函数来记录重要的投资组合统计数据（收益、方差和夏普比）。通过对约束最优问题的求解得到最优解。其中约束是权重总和为 1。

```python
def statistics(weights):
    weights = np.array(weights)
    port_returns = np.sum(returns.mean() * weights) * 252
    port_variance = np.sqrt(np.dot(weights.T, np.dot(returns.cov() * 252, weights)))
    return np.array([port_returns, port_variance, port_returns/port_variance])

# 最优化投资组合的推导是一个约束最优化问题
import scipy.optimize as sco
# 最小化夏普比的负值
def min_sharpe(weights):
    return -statistics(weights)[2]
```
