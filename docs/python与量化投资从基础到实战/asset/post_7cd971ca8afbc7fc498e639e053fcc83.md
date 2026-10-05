
![](img/0b89c6ed12810914fc2fb80a08af1fae_272_0.png)

# 图 6-13

双均线策略的特点已经在 6.5.1 节中讲到了，在此不再重复阐述。

接下来对该策略的回撤期进行归纳，这里用到了 gen_drawdown_table 函数：

```
returns = perf['returns']
returns.index = pd.to_datetime(returns.index)
gen_drawdown_table(returns)
```

|  | net drawdown in % | peak date | valley date | recovery date | duration |
|---|---|---|---|---|---|
| 0 | 31.3717 | 2016-07-13 | 2016-07-29 | NaT | NaN |
| 1 | 17.8365 | 2016-06-13 | 2016-06-24 | 2016-07-04 | 16 |
| 2 | 3.25517 | 2016-07-04 | 2016-07-06 | 2016-07-11 | 6 |
| 3 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 4 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 5 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 6 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 7 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 8 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |
| 9 | 0 | 2016-06-01 | 2016-06-01 | 2016-06-01 | 1 |

Python 与量化投资：从基础到实战

在如上所示的表中给出了不同的回撤期，例如从 2016 年 7 月 4 日至 2016 年 7 月 6 日，策略发生了回撤，在 2016 年 7 月 6 日之后策略重新赢利，直到 2016 年 7 月 11 日，组合累计收益率恢复至回撤前的水平。

我们可以调用 plot_drawdown_periods 函数更加直观地对如上所示的表进行展示，其中，参数 returns 为累计收益率序列，top 用于指定需要绘制多少个回撤期（从大到小排列）：

```
fig = plt.figure(figsize=(8, 4))
ax = fig.add_subplot(111)
ax = plot_drawdown_periods(returns, top=1, ax=ax)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_273_0.png)

接下来通过 plot_drawdown_underwater 函数绘制当前时间节点下的策略的回撤：

```
ax = plot_drawdown_underwater(perf['returns'])
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_273_1.png)

下面通过 plot_monthly_returns_heatmap 函数绘制策略每个月的收益。相对而言，对一些月度调仓的股票策略，使用该图进行分析更加有用：

```
ax = plot_monthly_returns_heatmap(returns)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_274_0.png)

下面通过 plot_monthly_returns_dist 函数绘制月度收益的分布图：

```
ax = plot_monthly_returns_dist(returns)
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_274_1.png)

Python 与量化投资：从基础到实战

是不是很方便？我们还可以通过查询优矿专业版的帮助文档，来获得更多的功能。

## 6.6 Smart Beta

Smart Beta（聪明贝塔，又称智慧型投资策略）是指在传统的指数投资基础上，通过系统性的方法对指数中选股和权重进行优化，以跑赢传统指数投资的策略。与传统的市值加权指数相比，在传统的指数中市值越高的个股将占有越大的权重，往往将指数投资者暴露于被高估的股票及投资集中的风险中。而 Smart Beta 与传统的指数编制方法不同，它通过对传统指数选股及权重的优化，在指数化被动管理的同时，相对于传统指数也能够取得一定的超额收益。

下面分别从基于权重优化和基于风险因子的角度进行详细介绍。

### 6.6.1 基于权重优化的 Smart Beta

本节详细介绍从权重优化角度来看各种配置方法的理论与实践，包括等权组合、最小方差组合、风险平价组合和最大多元化组合。

#### 1. Beta 简介

众所周知，Beta 在 CAPM 模型中衡量了相对于持有整个市场所带来的风险溢价（Risk Premium）的大小。我们通常用市场投资组合或市场指数基金来表示整个市场，市场指数通常都是市值加权（Market Capitalization Weighted），如果把市场指数换成按非市值加权的指数或投资组合，则得到的 Beta 就是 Smart Beta。下面分别从等权组合、最小方差组合、风险平价组合、最大多元化组合这 4 个角度进行权重优化。

#### 2. 原理与算法

1）等权重

每个成分股的权重都一样，权重的计算公式如下，其中，N 为成分股的个数：

$w_i = \frac{1}{N}$

2）最小方差

用于保证组合整体风险最小，可以用如下优化问题表示：

```
w* = min(1/2 ω'Ωω)
s.t.  w'1 = 1
ω_i ≥ 0
```

在上式中，Ω 为各资产的协方差矩阵，ω 为各资产的权重列向量。

3）风险平价

用于保证每个资产对组合整体风险的贡献都一样，可以用如下优化问题表示：

```
min f(ω) = Σ_{i=1}^N Σ_{j=1}^N [ω_i (Ωω)_i - ω_j (Ωω)_j]^2
s.t.  w'1 = 1
ω_i ≥ 0
```

在上式中，Ω 为各资产的协方差矩阵，ω 为各资产的权重列向量。

4）最大多元化

首先假设组合投资没有风险的功能，那么组合波动率就可以像收益率一样加权求得，但在实际情况下组合投资可以降低组合风险，那么最大多元化（也就是最大分散度）可以被定义为加权波动率与真实波动率的比值，求解过程就可以转换为如下优化问题：

```
max D(ω) = ω'σ / sqrt(ω'Ωω)
s.t.  w'1 = 1
ω_i ≥ 0
```

在上式中，Ω 为各资产的协方差矩阵，ω 为各资产的权重列向量，σ 为各资产的标准差列向量。

#### 3. 回测

1）定义相关函数

用于定义在研究过程中使用的关于读取数据、预处理数据及上面介绍的几种投资组合权重最优化的相关函数。

Python 与量化投资：从基础到实战

定义在实测中读取相关数据的函数如下：

```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('white')
from scipy.optimize import minimize
from cvxopt import matrix, solvers
from datetime import datetime as dt
from CAL.PyCAL import font
def get_covmat(tickers, date, periods):
    '''
    输入 tickers + 日期 + 过去天数，获得以此计算出来的年化协方差矩阵
    '''
    start_date = shift_date(date, periods)
    return_mat=DataAPI.MktEqudAdjGet(ticker=tickers, beginDate=start_date, endDate=date, field=u"ticker,tradeDate,closePrice", pandas="1")
    return_mat = return_mat.pivot(index='tradeDate',columns='ticker', values='closePrice').pct_change().fillna(0.0)
    return return_mat.cov()*250
```

定义输入协方差矩阵，得到不同优化方法下的权重配置的如下函数：

```
def get_smart_weight(cov_mat, method='min variance', wts_adjusted=False):
    '''
    功能：输入协方差矩阵，以得到不同优化方法下的权重配置
    输入：
        cov_mat pd.DataFrame，协方差矩阵，index 和 column 均为资产名称
        method 优化方法，可选的有 min variance、risk parity、max diversification、equal weight
    输出：
        pd.Series index 为资产名, values 为 weight
    PS:
        依赖 scipy package
    '''
    if not isinstance(cov_mat, pd.DataFrame):
        raise ValueError('cov_mat should be pandas DataFrame! ')
    omega = np.matrix(cov_mat.values)  # 协方差矩阵
```

```
# 定义目标函数
def fun1(x):
    return np.matrix(x) * omega * np.matrix(x).T

def fun2(x):
    tmp = (omega * np.matrix(x).T).A1
    risk = x * tmp
    delta_risk = [sum((i - risk)**2) for i in risk]
    return sum(delta_risk)

def fun3(x):
    den = x * omega.diagonal().T
    num = np.sqrt(np.matrix(x) * omega * np.matrix(x).T)
    return num/den

# 初始值 + 约束条件
x0 = np.ones(omega.shape[0]) / omega.shape[0]
bnds = tuple((0,None) for x in x0)
cons = ({'type':'eq', 'fun': lambda x: sum(x) - 1})
options={'disp':False, 'maxiter':1000, 'ftol':1e-20}

if method == 'min variance':
    res = minimize(fun1, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'risk parity':
    res = minimize(fun2, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'max diversification':
    res = minimize(fun3, x0, bounds=bnds, constraints=cons,
method='SLSQP', options=options)
elif method == 'equal weight':
    return pd.Series(index=cov_mat.index, data=1.0 / cov_mat.shape[0])
else:
    raise ValueError('method should be min variance/risk parity/max diversification/equal weight!!! ')

# 权重调整
if res['success'] == False:
    # print res['message']
    pass
wts = pd.Series(index=cov_mat.index, data=res['x'])
```

Python 与量化投资：从基础到实战

```
if wts_adjusted == True:
    wts = wts[wts >= 0.0001]
    return wts / wts.sum() * 1.0
elif wts_adjusted == False:
    return wts
else:
    raise ValueError('wts_adjusted should be True/False! ')
```

定义获取成分股的函数如下：

```
def get_idx_cons(idx, date):
    """
    功能：获取指数在某一天的成分股列表
    输入：
        idx 指数, xxxxxx 型 string, 000300 沪深300, 000016 上证50, 000905 中证500, 000906 中证800, 000001 上证综指
            可以为多个指数的组合，写法为 ['xxxxxx','xxxxxx']，此时返回的结果已经去除重复的 ticker 了!!!
        date yyyymmdd 型 string
    输出：
        list of tickers
    依赖:
        DataAPI: IdxConsGet
    """
    try:
        data = DataAPI.IdxConsGet(ticker=idx, intoDate=date, field='', pandas="1")['consTickerSymbol']
    except:
        raise ValueError('DataAPI.IdxConsGet 出错了!!! ')
    return list(set(data))
```

定义一个获取调仓日期列表的函数如下：

```
def get_dates(start_date, end_date, frequency='daily'):
    """
    功能：输入起始日期和频率，即可获得日期列表（daily 包括起始日，其余的都是位于起始日中间的）
    输入参数:
        start_date, 开始日期, 'xxxxxxxx' 形式
        end_date, 截止日期, 'xxxxxxxx' 形式
        frequency, 频率, daily 为所有交易日, daily1 为所有自然日, weekly 为每周最后
```

一个交易日，weekly2 为每隔两周，monthly为每月最后一个交易日，quarterly为每季最后一个交易日

输出参数：
获得 list 型日期列表，以 'xxxxxxxxx' 形式存储
PS：
要用到的 DataAPI.TradeCalGet!!!

```
data = DataAPI.TradeCalGet(exchangeCD=u"XSHG",beginDate=start_date,
endDate=end_date,field=u"calendarDate,isOpen,isWeekEnd,isMonthEnd,isQuarterEnd",pandas="1")
if frequency == 'daily':
    data = data[data['isOpen'] == 1]
elif frequency == 'daily1':
    pass
elif frequency == 'weekly':
    data = data[data['isWeekEnd'] == 1]
elif frequency == 'weekly2':
    data = data[data['isWeekEnd'] == 1]
    data = data[0:data.shape[0]:2]
elif frequency == 'monthly':
    data = data[data['isMonthEnd'] == 1]
elif frequency == 'quarterly':
    data = data[data['isQuarterEnd'] == 1]
else:
    raise ValueError('调仓频率必须为 daily/daily1/ weekly/weekly2 /monthly /quarterly!!! ')
    date_list = data['calendarDate'].values.tolist()
    return date_list
```

定义向前或向后移动 n 个交易日的函数如下：

```
def shift_date(date, n, direction='back'):
    '''
    功能：给定 date，获取该日期前、后 n 个交易日对应的交易日
    输入：
        date 'yyyymmdd' 类型字符串
        n 非负整数，取值区间为 (0,720)
        direction 方向，取值为 back/forward
    PS:
        get_dates()
    '''
    last_two_year = str(int(date[:4])-3) + '0101'
```
