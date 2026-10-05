
![](img/17116fd366c9823d0fab61da07c1a825_348_0.png)

图 10-16

通过图10-16，我们可以假定阶次p为2。现在来生成均值方程 AR(2)模型，并绘制残差a_t、a_t²的图形，示例代码如下：

```
order = (2,0)
mean_model = sm.tsa.ARMA(rtn,order).fit()
```

```
at = rtn - mean_model.fittedvalues
at2 = np.square(at)
plt.figure(figsize=(10,6))
plt.subplot(211)
plt.plot(at,label = 'at')
plt.legend()
plt.subplot(212)
plt.plot(at2,label='at^2')
plt.legend(loc=0)
```

绘制结果如图10-17所示。

![](img/17116fd366c9823d0fab61da07c1a825_349_0.png)

图 10-17

![](img/17116fd366c9823d0fab61da07c1a825_350_0.png)

图 10-17 （续）

下面来检验是否具有相关性，代码如下：

```
m = 10 # 我们检验10个自相关系数
acf, q, p = sm.tsa.acf(at2, n_lags=m, qstat=True)  ## 计算自相关系数及p-value
out = np.c_[range(1, m+1), acf[1:], q, p]
output=pd.DataFrame(out, columns=['lag', "AC", "Q", "P-value"])
output = output.set_index('lag')
output
```

得到的结果如图10-18所示。

| lag | AC | Q | P-value |
|---|---|---|---|
| 1.0 | 0.233668 | 13.432403 | 2.473151e-04 |
| 2.0 | 0.197474 | 23.065714 | 9.802658e-06 |
| 3.0 | 0.213674 | 34.391364 | 1.637922e-07 |
| 4.0 | 0.153189 | 40.236971 | 3.866477e-08 |
| 5.0 | 0.114327 | 43.506581 | 2.916797e-08 |
| 6.0 | 0.079023 | 45.075237 | 4.521836e-08 |
| 7.0 | 0.086284 | 46.953325 | 5.700257e-08 |
| 8.0 | 0.056257 | 47.755118 | 1.100363e-07 |
| 9.0 | 0.023511 | 47.895756 | 2.671212e-07 |
| 10.0 | 0.061421 | 48.859706 | 4.321588e-07 |

图 10-18

从图10-18可以看出，p-value小于显著性水平0.05，所以认为序列具有相关性，即具有ARCH效应。现在我们来判定ARCH模型的阶次，

代码如下：

```
fig = plt.figure(figsize=(10,5))
ax1=fig.add_subplot(111)
fig = sm.graphics.tsa.plot_pacf(at2,lags = 30,ax=ax1)
```

绘制结果如图10-19所示。

![](img/17116fd366c9823d0fab61da07c1a825_352_0.png)

图 10-19

根据图10-19，我们可以将阶次定为2阶（从第3个点开始，都没有超出范围）。所以最终的选择是均值方程为AR(2)模型，波动率模型为ARCH(2)模型。

下面来拟合模型，示例代码如下：

```
am = arch.arch_model(rtn,mean='AR',lags=2,vol='ARCH',p=2)
res = am.fit()
```

通过以下命令显示拟合模型的结果：

```
res.summary()
```

得到的结果如图10-20所示。

# AR - ARCH Model Results

| Dep. Variable: | y | R-squared: | 0.051 |
| Mean Model: | AR | Adj. R-squared: | 0.043 |
| Vol Model: | ARCH | Log-Likelihood: | 566.340 |
| Distribution: | Normal | AIC: | -1120.68 |
| Method: | Maximum Likelihood | BIC: | -1099.77 |
|  |  | No. Observations: | 241 |
| Date: | Tue, Aug 15 2017 | Df Residuals: | 235 |
| Time: | 17:16:28 | Df Model: | 6 |

图 10-20

# Mean Model

|  | coef | std err | t | P>|t| | 95.0% Conf. Int. |
|---|---|---|---|---|---|
| Const | 1.2506e-03 | 1.480e-03 | 0.845 | 0.398 | [-1.649e-03,4.150e-03] |
| y[1] | 0.1472 | 8.243e-02 | 1.786 | 7.404e-02 | [-1.431e-02, 0.309] |
| y[2] | -0.2075 | 0.101 | -2.063 | 3.913e-02 | [-0.405,-1.035e-02] |

# Volatility Model

|  | coef | std err | t | P>|t| | 95.0% Conf. Int. |
|---|---|---|---|---|---|
| omega | 3.5183e-04 | 6.173e-05 | 5.699 | 1.203e-08 | [2.308e-04,4.728e-04] |
| alpha[1] | 0.1767 | 0.133 | 1.331 | 0.183 | [-8.357e-02, 0.437] |
| alpha[2] | 0.2346 | 0.160 | 1.463 | 0.144 | [-7.977e-02, 0.549] |

图 10-20 （续）

虽然上面演示了ARCH模型的基本语法。但在实际应用中，我们通常更多地是使用GARCH模型来进行波动率预测。

## 10.10.4 GARCH模型

如果说ARCH模型对应着AR模型，那么GARCH模型对应的就是ARMA模型。GARCH模型是ARCH模型的推广形式，是由Bollerslev（1986年）提出来的。

对于收益率序列 r_t，令 a_t = r_t - μ_t，若 a_t 满足公式（10-8），则称 a_t 服从 GARCH(m,s)模型：

a_t = σ_t ε_t, σ_t^2 = α_0 + Σ_{i=1}^m α_i a_{t-i}^2 + Σ_{j=1}^s β_j σ_{t-j}^2    (10-8)

其中，ε_t 是均值为0、方差为1的独立同分布随机变量序列，α0>0, αi≥0, βj≥0, Σ_{i=1}^{max(m,s)} (α_i + β_j) < 1。

若 s=0，GARCH(m,s) 就简化成一个 ARCH(m) 模型了。α_i 和 β_j 分别称为 ARCH 参数和 GARCH 参数。

在实际应用中，GARCH 模型的阶不太容易确定，所以往往直接使用低阶的 GARCH 模型，比如 GARCH(1,1) 模型。

下面的示例仍然使用演示ARCH模型时所用的数据，那么均值方程也仍然可以选择AR(2)模型。拟合模型的示例代码如下：

```
am = arch.arch_model(rtn,mean='AR',lags=2,vol='GARCH')
res = am.fit()
```

可通过如下命令来查看拟合的结果：

```
res.summary()
```

得到的结果如图10-21所示。

# AR - GARCH Model Results

| Dep. Variable: | y | R-squared: | 0.051 |
| Mean Model: | AR | Adj. R-squared: | 0.043 |
| Vol Model: | GARCH | Log-Likelihood: | 567.101 |
| Distribution: | Normal | AIC: | -1122.20 |
| Method: | Maximum Likelihood | BIC: | -1101.29 |
|  |  | No. Observations: | 241 |
| Date: | Thu, Aug 17 2017 | Df Residuals: | 235 |
| Time: | 15:02:08 | Df Model: | 6 |

# Mean Model

|  | coef | std err | t | P>|t| | 95.0% Conf. Int. |
|---|---|---|---|---|---|
| Const | 1.3586e-03 | 1.451e-03 | 0.936 | 0.349 | [-1.485e-03,4.202e-03] |
| y[1] | 0.1545 | 6.930e-02 | 2.229 | 2.579e-02 | [1.867e-02, 0.290] |
| y[2] | -0.1931 | 7.459e-02 | -2.589 | 9.619e-03 | [-0.339,-4.693e-02] |

# Volatility Model

|  | coef | std err | t | P>|t| | 95.0% Conf. Int. |
|---|---|---|---|---|---|
| omega | 1.1949e-05 | 1.304e-11 | 9.161e+05 | 0.000 | [1.195e-05,1.195e-05] |
| alpha[1] | 0.0500 | 1.852e-02 | 2.700 | 6.928e-03 | [1.371e-02,8.629e-02] |
| beta[1] | 0.9300 | 1.614e-02 | 57.607 | 0.000 | [ 0.898, 0.962] |

图 10-21

画图的命令如下：

```
res.plot()
```

绘制的图形如图10-22所示。

# Standardized Residuals

![](img/17116fd366c9823d0fab61da07c1a825_360_0.png)

# Conditional Volatility

![](img/17116fd366c9823d0fab61da07c1a825_360_1.png)

# Standardized Residuals

![](img/17116fd366c9823d0fab61da07c1a825_360_2.png)

# Conditional Volatility

![](img/17116fd366c9823d0fab61da07c1a825_360_3.png)

图 10-22

# 第11章 数据源和数据库

## 11.1 数据来源

在进行量化分析的时候，最基础的工作是数据准备，即收集数据、清理数据、建立数据库。下面先讨论收集数据的来源，数据来源可分为两大类：免费的数据源和商业数据库。

免费数据源包括新浪财经、Yahoo财经等。这些数据源提供的接口比较复杂，不是很 好用。好消息是，Python中有对应的开源工具可以让数据获取变得简单。比如，TuShare能够获取新浪财经的数据，pandas-reader能够获取Yahoo Finance的数据。本章主要讨论TuShare和pandas-reader的用法。

商业数据库包括万得（Wind）、同花顺（iFind）、聚源等。商业数据库也分为两种，一种是软件终端，除了可以提供各种数据查询和可视化功能之外，还提供了接口以便从终端上提取数据。比如万得终端。终端的好处是价格便宜，简单易用。坏处是对提取的数据量有限制，而且数据的定制化功能很差，很多数据甚至都没有提供接口。另一种是落地数据库，通常是SQL数据库，所有数据直接落地，可以用SQL语句进行提取。落地数据库的好处是数据丰富，定制灵活，没有数据量限制；缺点是成本较高，而且技术门槛也相对较高。大部分人

使用的是第一种，即终端提供的接口。笔者曾经使用过第二种，现在主要也是使用第一种。本章就以Wind为例，讨论第一种数据库的用法。

## 11.2 TuShare

TuShare可以说是国内最早的Python开源财经数据接口项目，主要实现了对股票等金融数据从数据采集、清洗加工到数据存储的过程。TuShare返回的绝大部分的数据格式都是Pandas DataFrame类型，以便于用Pandas进行数据分析和可视化操作。

TuShare面市三年多以来不断更新迭代，在广度和深度等方面都得到了提升，现在最新版是Pro版，可针对的数据内容扩大到包含股票、基金、期货、债券、外汇等行业大数据，同时还包括了数字货币行情等区块链数据，它已成为全数据品类金融大数据平台，可以为各类金融投资和研究人员提供适用的数据和工具。Pro版数据更稳定、质量更好，它也是一个开放的、免费的平台，不带任何商业性质和目的。

TuShare的官方网站地址为： https://tushare.pro

### 11.2.1 TuShare安装

由于Anaconda并没有包含TuShare模块，所以需要自行安装，
TuShare具体包含如下三种安装方式。

- 方式1：执行命令pip install tushare。如果安装网络超时则可尝试国内pip源，如pip install tushare -i https://pypi.tuna.tsinghua.edu.cn/simple。
- 方式2：访问https://pypi.python.org/pypi/tushare/下载安装，执行命令python setup.py install。
- 方式3：访问https://github.com/waditu/tushare，将项目下载到本地，进入到项目的目录下，执行命令python setup.py install。

### 11.2.2 TuShare的Python SDK

TuShare是一个免费的数据接口项目，可用于提取数据。需要注意的是，TuShare的版本正在不断迭代，使用方法请以官网介绍为主，这里只做一个简单介绍。

导入TuShare，命令如下：

```
import tushare as ts
```

这里需要注意的是，TuShare的版本需要大于1.2.10。

设置token，命令如下：

```
ts.set_token('your token here')
```

以上方法只需要在第一次或者token失效后调用，即可完成调取TuShare数据凭证的设置，正常情况下不需要重复设置。也可以忽略此步骤，直接用pro_api('your token')语句完成初始化。

初始化pro接口的命令如下：

```
pro = ts.pro_api()
```

如果上一步骤ts.set_token('your token')无效或不想将token保存到本地，则可以在初始化接口里直接设置token，示例代码如下：

```
pro = ts.pro_api('your token')
```

下面以调取数据来获取交易日历信息为例进行说明，代码如下：

```
df = pro.trade_cal(exchange='', start_date='20180901',
end_date='20181001', fields=
'exchange,cal_date,is_open,pretrade_date', is_open='0')
```

或者：

```
df = pro.query('trade_cal', exchange='', start_date='20180901',
end_date=
'20181001',
fields='exchange,cal_date,is_open,pretrade_date', is_open='0')
```

调取结果如下：

|  | exchange | cal_date | is_open | pretrade_date |
|---|----------|----------|---------|---------------|
| 0 | SSE | 20180901 | 0 | 20180831 |
| 1 | SSE | 20180902 | 0 | 20180831 |
| 2 | SSE | 20180908 | 0 | 20180907 |
| 3 | SSE | 20180909 | 0 | 20180907 |
| 4 | SSE | 20180915 | 0 | 20180914 |
| 5 | SSE | 20180916 | 0 | 20180914 |
| 6 | SSE | 20180922 | 0 | 20180921 |
| 7 | SSE | 20180923 | 0 | 20180921 |
| 8 | SSE | 20180924 | 0 | 20180921 |
| 9 | SSE | 20180929 | 0 | 20180928 |
| 10 | SSE | 20180930 | 0 | 20180928 |
| 11 | SSE | 20181001 | 0 | 20180928 |
