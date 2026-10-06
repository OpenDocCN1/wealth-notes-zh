
## 14.5.1 Alpha（阿尔法）

投资中面临着系统性风险（Beta）和非系统性风险（Alpha），Alpha 是投资者获得与市场波动无关的回报。比如，投资者获得了 15%的回报，其基准获得了 10%的回报，那么 Alpha 或者价值增值的一部分就是 5%。

Alpha 的计算公式为：

```
Alpha = Rₚ − [R_f + βₚ(R_m − R_f)]
```

式中：
Rₚ——策略年化收益率；
Rₘ——基准年化收益率；
R_f——无风险利率（默认为 0.04）；
βₚ——策略 beta 值。

在回测详情页面中，单击左侧列表框中的 Alpha，就可以看到 MACD 指标量化策略的 Alpha 信息，如图 14.17 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_326_0.png)

> 提醒 如果 Alpha>0，策略相对于风险获得了超额收益；Alpha=0，策略相对于风险获得了适当收益；Alpha<0，策略相对于风险获得了较少收益。

## 14.5.2 Beta（贝塔）

Beta 表示投资的系统性风险，反映了策略对大盘变化的敏感性。例如，一个策略的 Beta 为 1.5，则大盘涨 1% 的时候，策略可能涨 1.5%，反之亦然；如果一个策略的 Beta 为 -1.5，说明大盘涨 1% 的时候，策略可能跌 1.5%，反之亦然。

Beta 的计算公式如下：

Beta=βₚ = Cov(Dₚ, Dₘ) / Var(Dₘ)

式中：

+   Dₚ——策略每日收益；
+   Dₘ——基准每日收益；
+   Cov(Dₚ,Dₘ)——策略每日收益与基准每日收益的协方差；
+   Var(Dₘ)——基准每日收益的方差。

当 Beta<0，投资组合和基准的走向通常反方向，如空头头寸类。如果 Beta=0，投资组合和基准的走向没有相关性，如固定收益类。0<Beta<1，投资组合和基准的走向相同，但是比基准的移动幅度更小。如果 Beta=1，投资组合和基准的走向相同，并且和基准的移动幅度贴近。如果 Beta>1，投资组合和基准的走向相同，但是比基准的移动幅度更大。

在回测详情页面中，单击左侧列表框中的 Beta，就可以看到 MACD 指标量化策略的 Beta 信息，如图 14.18 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_327_0.png)

图 14.18 MACD 指标量化策略的 Beta 信息

## 14.5.3 Sharpe（夏普比率）

Sharpe 表示每承受一单位总风险会产生多少的超额报酬，可以同时对策略的收益与风险进行综合考虑。

Sharpe 的计算公式为

Sharpe Ratio = (R_p - R_f) / σ_p

式中：

R_p —— 策略年化收益率；
R_f —— 无风险利率（默认为 0.04）；
σ_p —— 策略收益波动率。

在回测详情页面中，单击左侧列表框中的 Sharpe，就可以看到 MACD 指标量化策略的 Sharpe 信息，如图 14.19 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_328_0.png)

## 14.5.4 Sortino（索提诺比率）

Sortino 表示每承担一单位的下行风险，将会获得多少超额回报。

Sortino 的计算公式为：

Sortino Ratio = (R_p - R_f) / σ_pd

式中：

R_p —— 策略年化收益率；
R_f —— 无风险利率（默认为 0.04）；
σ_pd —— 策略下行波动率。

在回测详情页面中，单击左侧列表框中的 Sortino，就可以看到 MACD 指标量化策略的 Sortino 信息，如图 14.20 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_329_0.png)

图 14.20 MACD 指标量化策略的 Sortino 信息

## 14.5.5 Information Ratio（信息比率）

Information Ratio 是用来衡量单位超额风险带来的超额收益。信息比率越大，说明该策略单位跟踪误差所获得的超额收益越高。因此，信息比率较大的策略其表现要优于信息比率较低的基准。合理的投资目标应该是投资者在承担适度风险下，尽可能追求高信息比率。

Information Ratio 的计算公式为：

Information Ratio = (R_p - R_m) / σ_t

式中：

R_p——策略年化收益率；
R_m——基准年化收益率；
σ_t——策略与基准每日收益差值的年化标准差。

在回测详情页面中，单击左侧列表框中的 Information Ratio，就可以看到 MACD 指标量化策略的 Information Ratio 信息，如图 14.21 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_329_1.png)

图 14.21 MACD 指标量化策略的 Information Ratio 信息

## 14.5.6 Volatility（策略波动率）

策略波动率用来测量策略的风险性，波动越大代表策略风险越高。

Volatility 的计算公式为：

```
Volatility = σ_p = √(250/(n-1) Σ_{i=1}^n (r_p - r̄_p)^2)
```

式中：

+   r_p——策略每日收益率；
 r̄_p——策略每日收益率的平均值=1/n Σ i=1n r_p；
 n——策略执行天数。

在回测详情页面中，单击左侧列表框中的 Volatility，就可以看到 MACD 指标量化策略的 Volatility 信息，如图 14.22 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_330_0.png)

## 14.5.7 Benchmark Volatility（基准波动率）

Benchmark Volatility 用来测量基准的风险性，波动越大代表基准风险越高。

Benchmark Volatility 的计算公式为：

```
Benchmark Volatility = σ_m = √(250/(n-1) Σ_{i=1}^n (r_m - r̄_m)^2)
```

式中：

+   r_m——基准每日收益率；
 r̄_m——基准每日收益率的平均值=1/n Σ i=1n r_m；
 n——基准执行天数。

在回测详情页面中，单击左侧列表框中的 Benchmark Volatility，就可以看到 MACD 指标量化策略的 Benchmark Volatility 信息，如图 14.23 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_331_0.png)

图 14.23 MACD 指标量化策略的 Benchmark Volatility 信息

## 14.5.8 Max Drawdown（最大回撤）

Max Drawdown 描述策略可能出现的最糟糕情况和最极端可能的亏损情况。

Max Drawdown 的计算公式为：

```
Max Drawdown = Max (Pₓ − Pᵧ) / Pₓ
```

式中：Pₓ,Pᵧ——策略某日股票和现金的总价值。

在回测详情页面中，单击左侧列表框中的 Max Drawdown，就可以看到 MACD 指标量化策略的 Max Drawdown 信息，如图 14.24 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_331_1.png)

图 14.24 MACD 指标量化策略的 Max Drawdown 信息

## 学习心得

## 第 15 章

## Python 量化交易策略的因子分析技巧

在量化交易中，因子就是量化选股或量化择时的标准。例如，如果认为净利润同比增长率高的公司就是好公司，那就把净利润同比增长率大于 50%的股票选出来。这个净利润同比增长率大于 50%就是因子。本章首先讲解因子的类型和作用；然后讲解因子分析的实现代码、因子分析的结果；最后讲解因子在研究和回测中的使用和基本面因子应用实例。

## 15.1 因子分析概述

下面先来讲解因子的类型；然后讲解因子分析的作用。

## 15.1.1 因子的类型

因子在量化交易中可以分为两类：分别是量化选股因子（好股票的标准）和量化择时因子（好股票买卖的标准）。量化选股因子，大多为财务指数，如营业利润率、销售净利率、营业收入环比增长率等；量化择时因子大多为技术指标，如均线、换手率、波动率等。

## 15.1.2 因子分析的作用

JoinQuant（聚宽）量化交易平台提供了因子分析功能，这样就方便投资者测试自己的量化因子，相比之前只能在回测模块中一个一个测试每个指标来说，因子分析系统规避了回测的噪声、复杂的资产配置和交易系统产生的导致指标失真的问题。

## 15.2 因子分析的实现代码

在量化交易中，要实现因子分析功能，需要自定义因子的类，继承 Factor 类，并实现 calc 方法，代码如下：

```
class MA10(Factor):
    name = 'ma10'
    max_window = 10
    dependencies = ['close']
    def calc(self, data):
        return data['close'][-10:].mean()
```

## 15.2.1 因子分析中变量的含义

在自定义类中，有 3 个变量，分别是 name、max_window、dependencies。

+   1. name

name 是因子的名称。在给因子命名时，必须符合 Python 中变量的命名规则，即名称只能由字母、数字和下画线组成，并且第一个字符不能是数字，另外不能与 Python 中的关键字相同，也不能与基础因子冲突。

## 15.2.2 因子分析中可以使用的基础因子

2. max_window
max_window 是用来获取数据的最长时间窗口。需要注意的是，返回的是日线级别的数据。

3. dependencies
dependencies 是用来设置依赖的基础因子名称。

在因子分析中，可以使用的基础因子很多，如价量因子、财务数据因子、行业因子、概念因子、指数因子、资金流因子，如图 15.1 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_335_0.png)

图 15.1 因子分析中可以使用的基础因子

1. 价量因子
价量因子是指利用 get_price()函数可以取到的价量信息，如 open（开盘价）、close（收盘价）、high（最高价）、low（最低价）、volume（成交量）、money（成交金额）。

2. 财务数据因子
财务数据因子是指当日可以看到的最新单季财务指标，如 pe_ratio（动态市盈率）、turnover_ratio（换手率）、pb_ratio（市净率）、market_cap（股票的总市值）、circulating_market_cap（股票的流通市值）等。

3. 行业因子
行业因子包含证监会行业分类，聚宽一、二级行业分类以及申万一、二、三级行业分类，如 A01（农业）、A02（林业）、B06（煤炭开采和洗选业）、B07（石油和天然气开采业）、C36（汽车制造业）等。

返回值是一个哑变量，如果某股票属于某行业，则返回 1；否则，返回 0。

> 提醒 哑变量，又称虚设变量、名义变量，是量化了的质变量，通常取值为 0 或 1。

# 4. 概念因子

概念因子，即概念板块代码，如 GN028（智能电网）、GN030（物联网）、GN092（高端装备制造）、GN181（一带一路）等。

返回值是一个哑变量，如果某股票属于某个概念，则返回 1；否则，返回 0。

# 5. 指数因子

指数因子，即指数代码，如 000001.XSHG（上证指数）、000002.XSHG（A 股指数）、000003.XSHG（B 股指数）、000006.XSHG（地产指数）等。

返回值是一个哑变量，如果某股票属于某个指数，则返回 1；否则，返回 0。

# 6. 资金流因子

资金流因子，即 get_money_flow() 函数查询的数据。可以使用的字段包括 change_pct（涨跌幅）、net_amount_main（主力净额）、net_pct_main（主力净占比）、net_amount_xl（超大单净额）、net_pct_xl（超大单净占比）、net_amount_l（大单净额）、net_pct_l（大单净占比）、net_amount_m（中单净额）、net_pct_m（中单净占比）、net_amount_s（小单净额）、net_pct_s（小单净占比）。

## 15.2.3 calc 的参数及返回值

在 calc 中，可以通过 data 参数获取 max_window 和 dependencies 定义的数据。参数 data 是一个 dict 对象，其 key 属性是 dependencies 中的因子名称，value 属性是 pandas.DataFrame。

DataFrame 的 column 是股票代码，可以选择沪深 300 作为股票池。

DataFrame 的 index 是一个时间序列，结束时间是当前时间，长度是 max_window。

calc 保证返回 pandas.Series，其中 index 属性是股票代码，value 属性是因子值。

## 15.3 因子分析的结果

前面讲解的因子分析的基础知识及实现代码，下面利用聚宽 JoinQuant 量化交易平台新建因子，并查看因子分析的结果。

## 15.3.1 新建因子

在浏览器的地址栏中输入 https://www.joinquant.com，然后按 Enter 键，就进入聚宽 JoinQuant 量化交易平台的首页页面。

在菜单栏中选择“我的策略”→“单因子分析”命令，进入“我的因子”页面，如图 15.2 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_337_0.png)

图 15.2 “我的因子” 页面

在“我的因子”页面中，单击“新建因子”按钮，就新建一个因子，然后编写 Python 代码，具体如下：

```
from jqfactor import Factor
class MONEY(Factor):
    # 设置因子名称
    name = 'ma'
    # 设置获取数据的时间窗口长度
    max_window = 1
    # 设置依赖的数据
    dependencies = ['money']
    # 计算因子的函数，需要返回一个 pandas.Series, index 是股票代码, value 是因子值
    def calc(self, data):
        ma = data['money'].mean()
        return zscore(ma)
# 标准化函数
def zscore(series):
    std = series.std()
    mean = series.mean()
    return (series - mean)/std
```

第一行代码：from jqfactor import Factor，是指从 jqfactor 模块中导入 Factor 类。其他代码与前面讲过的代码基本相同，这里不再多说。

下面设置因子分析的开始时间和结束时间，在这里都采用默认设置，即开始时间是 2017 年 10 月 1 日，结束时间是 2018 年 4 月 1 日。还要设置股票池，这里股票池要么是沪深 300，要么是中证 500，如图 15.3 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_338_0.png)

图 15.3 设置因子分析的开始时间、结束时间和股票池

接下来就可以编译运行代码，即单击“编译运行”按钮即可。在编译运行过程中，可以查看“日志”，即单击按钮。在“日志”中可以看到因子分析过程中的提示信息，如图 15.4 所示。

```
from jqfactor import Factor
class MONEY(Factor):
    # 设置因子名称
    name = "ma"
    # 设置采集数据的时间窗口长度
    max_window = 1.
    # 设置依赖的数据
    dependencies = ['money']
    # 计算因子的函数，需要返回一个 pandas.Series, index
    def calc(self, data):
        ma = data['money'].mean()
        return zscore(ma)

# 标准化函数
def zscore(series):
    std = series.std()
    mean = series.mean()
    return (series - mean)/std
```

![](img/261a0e647c121c3aa8af17bcacd0d7ae_338_1.png)

图 15.4 查看日志

编译运行完成后，单击按钮，就可以看到分位数累积收益图表信息，如图 15.5 所示。

需要注意的是，为了测试因子是否有效，往往会分层测试：将样本排序后划分为 5 类，每一类称为一个分位数，同时构建一个策略组合，分别测试其收益。

这里测试了股市中“热钱效应”的因子（money），会发现该效应短时间内预测效应较强，即单日成交额较大的股票在未来短时间内上涨的可能性更大。

图 15.5 分位数累计收益图表信息

## 15.3.2 收益分析

单击分位数累计收益图表信息上方的“分析”按钮，就开始进行因子分析。因子分析完成后，首先看到的是收益分析，如图 15.6 所示。

图 15.6 收益分析

收益分析包括 18 种，分别是各分位数平均收益、1 天分位数累计收益、1 天多空组合收益、5 天分位数累计收益、5 天多空组合收益、10 天分位数累计收益、10 天多空组合收益、房地产指数平均收益、日常消费指数平均收益、能源指数平均收益、金融指数平均收益、信息技术指数平均收益、医疗保健指数平均收益、公用事业指数平均收益、材料指数平均收益、电信服务指数平均收益、工业指数平均收益、可选消费指数平均收益。

+   （1）各分位数平均收益：表示持仓 1 天后，各分位数可以获得的平均收益。
（2）1 天分位数累计收益：表示持仓 1 天后，各分位数持仓收益的累计值。

```
from jqfactor import Factor
class MONEY(Factor):
    # 设置因子名称
    name = "m..."
    # 设置交易日收益的计算逻辑
    ...
    def calc(self, data):
        ...
    # 标准化和处理
    def zscore(series):
        ...
```

![](img/261a0e647c121c3aa8af17bcacd0d7ae_339_0.png)

![](img/261a0e647c121c3aa8af17bcacd0d7ae_339_1.png)
