
order_to(stock, 0)

执行上述代码，即可得到如图 17-1 所示的图形。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_181_0.png)

![](img/123c8fbcf9c4e19d1d2636062a3e4340_181_1.png)

## 练习题

对本章中的例题数据，使用 Python 重新操作一遍。

# 第18章

## Alpha对冲模型的 Python应用

前面介绍了量化金融投资的基本知识，本章主要用具体的实例来介绍如何在优矿平台上建立 Alpha 对冲模型。

## 18.1 Alpha对冲模型

假设市场完全有效，那么根据资本资产定价模型有

$$R_{e}=R_{f}+\beta_{e}(R_{m}-R_{f})$$

式中，$R_{e}$ 表示股票收益，$R_{f}$ 表示无风险收益率，$R_{m}$ 表示市场收益，$\beta_{e}$ 表示股票相比于市场的波动程度，用以衡量股票的系统性风险。

遗憾的是，市场并非完全有效，个股仍存在 Alpha（超额收益）。根据 Jensen 对 Alpha 的定义：$\alpha_{e}=R_{e}-[R_{f}+\beta_{e}(R_{m}-R_{f})]$，除掉被市场解释的部分，超越市场基准的收益即为个股 Alpha。

在实际中，股票的收益是受多方面因素影响的，比如经典的 Fama-French 三因素就告诉我们，用市值大小、估值水平以及市场因子就能够解释股票收益，而且低市值、低估值能够获取超额收益。因此，可以通过寻找能够获取 Alpha 的驱动因子来构建组合。

假设已知哪些因子能够获取超额收益，就可以根据这些因子构建股票组合（例如持有低市值、低估值的股票）。股票组合在理论上是能够获取超额收益的，简单来说就是，股票组合的累计收益率应该大于基准（如沪深 300 指数）累计收益率，而且两者的差应该呈扩大的趋势。

由于股票组合的涨跌是未知的，能够确保的是股票组合与基准的收益差在不断扩大，那么持有股票组合，做空基准，对冲获取稳定的差额收益（Alpha 收益），这就是所谓的市场中性策略。

## 18.2 优矿平台的“三剑客”

针对上述研究流程，优矿平台提供了从金融大数据、模型的研究开发到实盘交易和组合管理的全程服务。

- （1）DataAPI。提供近 300 个高质量的因子数据（基本面因子、技术面因子和大数据因子），为模型提供了充足的原材料，并为用户自己研究因子提供了基础。
- （2）RDP。提供了标准的因子到信号的处理函数（去极值、中性化、标准化），还提供了功能强大的组合构建函数。
- （3）Quartz。提供了标准的、更贴近实际的回测框架，可以一键查看对冲模型历史表现。

## 18.3 优矿平台对冲模型实例

回测框架和基础工作如下：

- （1）回测区间从 2011 年 8 月 1 日至 2015 年 8 月 1 日，基准为沪深 300 指数，策略为每月第一个交易日开盘之后建仓。
- （2）因子选取净利润增长率(NetProfitGrowRate)、权益收益率(ROE)和相对强弱指标(RSD)。
- （3）因子到信号的处理用到了去极值(winzorize)、中性化(neutralize)和标准化(standardize)处理。
- （4）组合构建用到了 RDP 中的 simple_long_only()。

**注意**：关于函数的详细使用说明，可以新建 cell，输入函数名和问号(？)，即可打开 API 使用文档。例如，运行下面的代码便可以得到 simple_long_only 的使用说明：

```python
simple_long_only?
from CAL.PyCAL import *
import numpy as np
from pandas import DataFrame
start = '2011 - 08 - 01'                    # 回测起始时间
end = '2015 - 08 - 01'                      # 回测结束时间
benchmark = 'HS300'                         # 策略基准
universe = set_universe('HS300')           # 股票池, 支持股票和基金
capital_base = 10000000                    # 初始资金
freq = 'd'
# 策略类型。'd'表示日间策略, 使用日线回测; 'm'表示日内策略, 使用分钟线回测
# refresh_rate = 1
# 调仓周期, 表示执行 handle_data 的时间间隔。若 freq = 'd', 则调仓周期的单位为交易日; 若
# freq = 'm', 则调仓周期的单位为分
# 构建日期列表
data = DataAPI.TradeCalGet(exchangeCD = "XSHG", beginDate = u"20110801", endDate = u"20150801",
field = ['calendarDate', 'isMonthEnd'], pandas = "1")
data = data[data['isMonthEnd'] == 1]
date_list = data['calendarDate'].values.tolist()
cal = Calendar('China.SSE')
period = Period(' - 1B')
def initialize(account):                     # 初始化虚拟账户状态
    pass
def handle_data(account):                   # 每个交易日的买入卖出指令
    today = account.current_date
    today = Date.fromDateTime(account.current_date)  # 向前移动一个工作日
    yesterday = cal.advanceDate(today, period)
    yesterday = yesterday.toDateTime()
    if yesterday.strftime('%Y - %m - %d') in date_list:
        # 净利润增长率
        NetProfitGrowRate = DataAPI.MktStockFactorsOneDayGet(tradeDate = yesterday
                    .strftime('%Y%m%d'), secID = account.universe,field= u"secID,NetProfitGrowRate", pandas = "1")
NetProfitGrowRate.columns = ['secID', 'NetProfitGrowRate']
NetProfitGrowRate['ticker'] = NetProfitGrowRate['secID'].apply(lambda x: x[:6])
NetProfitGrowRate.set_index('ticker', inplace =True)
ep = NetProfitGrowRate['NetProfitGrowRate'].dropna().to_dict()
signal_NetProfitGrowRate = standardize(neuralize(winsorize(ep),
yesterday.strftime('%Y%m%d')))
# 对因子进行去极值、中性化和标准化处理的信号
# 权益收益率
ROE = DataAPI.MktStockFactorsOneDayGet(tradeDate = yesterday.strftime('%Y%m%d'),
secID = account.universe,field = u"secID,ROE",pandas = "1")
ROE.columns = ['secID', 'ROE']
ROE['ticker'] = ROE['secID'].apply(lambda x: x[:6])
ROE.set_index('ticker', inplace =True)
ep = ROE['ROE'].dropna().to_dict()
signal_ROE = standardize(neutralize(winsorize(ep),yesterday.strftime('%Y%m%d')))
# 对因子进行去极值、中性化和标准化处理的信号
# 相对强弱指标
RSI = DataAPI.MktStockFactorsOneDayGet(tradeDate = yesterday.strftime('%Y%m%d'),
secID = account.universe,field = u"secID,RSI",pandas = "1")
RSI.columns = ['secID', 'RSI']
RSI['ticker'] = RSI['secID'].apply(lambda x: x[:6])
RSI.set_index('ticker', inplace =True)
ep = RSI['RSI'].dropna().to_dict()
if len(ep) == 0 :
    return
    signal_RSI = standardize(neutralize(winsorize(ep),yesterday.strftime('%Y%m%d')))
# 对因子进行去极值、中性化和标准化处理的信号
# 构建组合 score 矩阵
weight = np.array([0.4, 0.3, 0.3])              # 信号合成，各因子权重
Total_Score = DataFrame(index = RSI.index, columns = ['NetProfitGrowRate','ROE',
'RSI'], data = 0)
Total_Score['NetProfitGrowRate'][signal_NetProfitGrowRate.keys()] =
    signal_NetProfitGrowRate.values()
Total_Score['ROE'][signal_ROE.keys()] = signal_ROE.values()
Total_Score['RSI'][signal_RSI.keys()] = signal_RSI.values()
Total_Score['total_score'] = np.dot(Total_Score, weight)
total_score = Total_Score['total_score'].to_dict()
wts = simple_long_only(total_score, today.strftime('%Y%m%d'))
# 调用组合构建函数，组合构建综合考虑各因子大小、行业配置等因素，默认返回前 30 % 的股票
# 找载体，将 ticker 转化为 secID
RSI['wts'] = np.nan
RSI['wts'][wts.keys()] = wts.values()
RSI = RSI[~np.isnan(RSI['wts'])]
RSI.set_index('secID', inplace =True)
RSI.drop('RSI', axis =1, inplace =True)
# 先卖出
sell_list = account.valid_secpos
for stk in sell_list:
    order_to(stk, 0)
# 再买入
buy_list = RSI.index
total_money = account.referencePortfoliolValue
prices = account.referencePrice
for stk in buy_list:
	if np.isnan(prices[stk]) or prices[stk] == 0:
		# 因停牌或还未上市等原因不能交易
		continue
	order(stk, int(total_money * RSI.loc[stk]['wts'] / prices[stk] / 100) * 100)
else:
	return
```

运行上述代码,得到如图 18-1 所示的结果。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_185_0.png)

接下来要绘制股票组合和基准的累计收益率,得到 Alpha 收益,看看效果如何。代码如下:

```python
((bt['portfolio_value']/bt['portfolio_value'][0] - 1) - ((1 + bt['benchmark_return'])
	.cumprod() - 1)).plot(figsize=(14, 7))
```

运行上述代码,得到如图 18-2 所示的结果。

![](img/123c8fbcf9c4e19d1d2636062a3e4340_185_1.png)

从图 18-2 可以看到，在净利润增长率、权益收益率、相对强弱指标这 3 个因子驱动下的 Alpha 收益相对来说还是比较稳定的，由于有对冲，策略是市场中性的，不论市场涨跌，对收益是没有影响的（当然排除一些极端情况，例如所有股票收益没有任何差异性，又如流动性危机）。

## 练习题

对本章中的例题数据，使用 Python 重新操作一遍。

# 第 19 章 Signal 框架下的 Alpha 量化金融投资策略的 Python 应用

第 18 章介绍了如何在优矿平台上建立 Alpha 对冲模型，本章将重点介绍 Alpha 对冲模型的实战价值。本章的内容如下：

- （1）为什么选择 Alpha 对冲模型。
- （2）在优矿平台上构建 Alpha 对冲模型的神器——Signal 框架。
- （3）典型公募基金团队如何构建自己的 Alpha 对冲模型。
- （4）如何在优矿平台上一个人击败一家公募基金团队。

## 19.1 为什么选择 Alpha 对冲模型

### 1. 有效市场假说

20 世纪 70 年代，尤金·法玛提出了有效市场假说，根据市场的不同情况，又可以分为 3 种不同的状态：弱式有效、半强式有效和强式有效。

在弱式有效下，市场价格已充分反映出所有过去历史的股票价格信息，这种情况下，股票价格的技术分析失去了作用，基本面分析还能帮助投资者获得超额收益。

从现有的情况看，成熟的美国市场也仅仅处于弱式有效和半强式有效之间，众多学者研究表明，A 股市场由于散户参与量大；目前处于无效和弱式有效之间，价格和价值往往偏差较大，非常适合用 Alpha 对冲模型来投资。

### 2. 主动投资与被动投资

究竟是主动投资好还是被动投资好？主动投资一定会比被动投资能够获得更多的收益吗？这是很难回答的问题。

有众多学者做过相关研究，大多学者发现，从长期来看，被动投资甚至比主动投资表现还要好（虽然总有那么一小部分主动投资产品的表现是超过基准的），主动投资并不一定能带来超额收益。这里不一一列举学者们的文献，只介绍一下近几年来风靡美国的机器人投资顾创业公司，比如 Wealffront，它们的兴起就是从实际上证明有时候被动投资并不像大家所想的那么不堪一击。

Alpha 对冲模型虽然属于主动投资范畴，但从风险和收益的角度来看，它更像有主动管理的被动投资，Alpha 对冲模型的目标就是稳健地获取相对于基准指数的超额收益，例如每天比基准指数多 0.1% 的收益。

主动管理的被动投资，低风险下稳健的超额收益，这也是 Alpha 对冲模型的优势之一。

### 3. Alpha 对冲模型自身的优势

Alpha 对冲模型自身有以下优势：
- （1）从量化产品的角度来看，Alpha 对冲模型也有其独到的优势。
- （2）市场容量大，冲击成本小，Alpha 对冲模型本身并不建立在严格的假设基础之上，投资方法成熟，盈利可持续性强。

### 4. 量化 1.0 v.s. 量化 2.0

传统的量化 1.0 主要依靠主信号＋约束条件的办法，目前的国内的友商和国外的友商也只能做到这点。

量化 2.0 时代（目前大部分市场上开发真实产品的量化基金的研发系统）已经把系统的量化研究做得更细致，包含信号研发、信号组合与模型构建、风险模型以及组合管理。

优矿平台能够很好地支持量化 1.0 和量化 2.0，且基于海量金融大数据，可以使一个人完成一个量化基金团队可以做到的事情，甚至一个人也可以战胜 Bridgewater 和 TwoSigma。

## 19.2 在优矿平台上构建 Alpha 对冲模型的神器——Signal 框架

### 1. Alpha 对冲模型构建方法

每次在 handle_data 中通过 DataAPI 获取数据，然后根据数据构建信号、合成信号。
本节以经典的 Fama-French 三因子为例，采用了估值因子（市盈率，即 PE）和市值因子（对数流通市值，即 LFLO）。
不失一般性，采用等权的方式对因子进行加权。

代码如下。

```python
import numpy as np
import pandas as pd
start = '2011-01-01'
end = '2016-01-01'
benchmark = 'HS300'
universe = set_universe('HS300')
capital_base = 10000000
freq = 'd'
refresh_rate = 20
def initialize(account):
    pass
def handle_data(account):
    #获取上一个交易日
    yesterday = account.previous_date.strftime('%Y%m%d')
    #市盈率
    PE = DataAPI.MktStockFactorsOneDayGet(tradeDate = yesterday, secID = account.universe,
                                           field= u"secID,PE", pandas ="1").set_index('secID')
    PE = 1.0 / PE
    ep = PE['PE'].dropna().to_dict()
    signal_PE = pd.Series(standardize(neutralize(winsorize(ep),yesterday)))
    #对数流通市值
    LFLO = DataAPI.MktStockFactorsOneDayGet(tradeDate = yesterday, secID = account.universe,
```
