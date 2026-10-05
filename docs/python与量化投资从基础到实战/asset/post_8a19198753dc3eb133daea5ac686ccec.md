
# Python 与量化投资：从基础到实战

```
# 加上后缀，使得下面的行情 API 可以调用
symbol_history_list = [ ]
for symbol in index_symbol: # 该 API 每次只能调用一个 indexID
    df_daily_industry_symbol = DataAPI.MktIdxdGet(beginDate='2015-01-01', endDate='2018-01-23', ticker=symbol[:6], field=u"ticker, tradeDate, closeIndex")
    symbol_history_list.append(df_daily_industry_symbol)
    df_daily_industry_symbol = pd.concat(symbol_history_list,axis=0)
# 将获取的行业数据汇总
    df_daily_industry_unstack = df_daily_industry_symbol.set_index(['tradeDate','ticker']).unstack()['closeIndex'] # 将所有行业行情数据转为一张二维表
    return df_daily_industry_unstack
    df_daily_industry_unstack = get_sw_ind_quotation()
    df_daily_industry_unstack.head() # 如上我们便得到了申万一级行业至今的行情数据
```

接下来，将日线的行情数据转化为周线的行情数据，因为在第 1 天的行情中，有的一些行业存在缺失值，所以我们将第 1 天的行情剔除。定义月行情为每月的第 1 个交易日到下个月的第 1 个交易日：

```
df_daily_industry_unstack = df_daily_industry_unstack.iloc[1:]
# 去掉第 1 期基期
df_daily_industry_unstack['tradeDate'] = df_daily_industry_unstack.index
# 加入 tradeDate 列，方便做 map
def getMonthlyIndex(df_index):  # 得到月线行情
# 将 tradeDate 列转化为时间格式
    df_index['tradeDate'] = df_index['tradeDate'].map(lambda x:parse(x))
    df_index['year_month']=df_index['tradeDate'].map(lambda x:(x.year,x.month)) # 得到（年，月）用于筛选
    return df_index.groupby(['year_month']).head(1)
# 返回每个月第 1 个交易日的行情
df_monthly_industry_unstack = getMonthlyIndex(df_daily_industry_unstack[:])
df_monthly_industry_unstack = df_monthly_industry_unstack.sort_values(['tradeDate']) # 按 tradeDate 列排序
```

这样，我们便得到了申万一级行业每个月第 1 个交易日的行情数据。

## 3. 计算月度行业收益率排序矩阵

接下来计算月度收益率及其排名，通过调用 Series 的内置函数 pct_change()就可以很方便地计算收益率；通过调用 rank()函数按行排序就可以计算排名，默认为从小到大：

```
del df_monthly_industry_unstack['tradeDate']
# 删除 tradeDate 列和 year_month 列，方便后面调用后缀函数计算行业收益率
del df_monthly_industry_unstack['year_month']
df_monthly_industry_return = df_monthly_industry_unstack.pct_change(axis=0) # 调用 pct_change() 计算收益率
df_monthly_industry_return = df_monthly_industry_return.dropna(how='all')
# 删除全是缺失值的第 1 行
df_monthly_industry_return_rank = df_monthly_industry_return.rank(axis=1)
# 按行做排序
```

这样，我们便得到了申万一级行业每期收益率的排序情况。

## 4. 行业月度收益率相关性分析

获取申万一级行业指数代码，一共有 28 个：

```
index_symbol = DataAPI.IndustryGet(industryVersion=u"SW", industryVersionCD=u"", industryLevel=u"1", isNew=u"1", field=u"", pandas="1")['indexSymbol'].tolist()
def get_corr(ind1, ind2, df_ind):
    """
    返回行业 1 与行业 2 的 1 阶滞后的相关系数
    Args:
        ind1 (str): 行业指数代码
        ind2 (str): 行业指数代码
        df_ind (DataFrame): 所有行业指数的月度收益表
    Returns:
        numpy.float64: 行业 1 与行业 2 的 1 阶滞后的相关系数
    Examples:
        >>ind_corr=get_corr('801760','801150',df_monthly_industry_return_rank)
    """
    x = df_ind[ind1].iloc[0:-1].values
    y = df_ind[ind2].iloc[1:].values
    return np.corrcoef(x,y)[0][1]   #返回两组数据的皮尔森相关系数
predict_corr = { }
for item in itertools.product(index_symbol,repeat=2):
#列表中的每个元素和剩余所有的元素进行配对
    predict_corr[item]=get_corr(item[0],item[1],df_monthly_industry_return_rank) # 得到行业收益率排名的相关系数字典
predict_corr = pd.Series(predict_corr) # 将字典转化为序列
predict_corr.hist() # 做出相关系数的频数直方图
```

![](img/0b89c6ed12810914fc2fb80a08af1fae_215_0.png)

可以看出，行业与行业的一阶滞后的相关性并不高，大部分维持在 0 附近，我们要找出那些相关系数在统计上显著的两个行业。根据统计公式，相关系数不等于 0 的显著性 $t$ 的统计量为：

$$t = \frac{r\sqrt{n-2}}{1 - r^3}, df = n - 2$$

查阅 $t$ 的分布表，$t_{(34)01}=2.728$，反推出 $|r|=0.43$，即当 $|r|>0.43$ 时，在 99%置信水平上是统计显著的。所以我们筛选出大于 0.43 或小于-0.43 时的行业相关性：

```
filter_corr = predict_corr[(predict_corr>0.43) | (predict_corr<-0.43)]
# 筛选相关系数大于0.43或小于-0.43时的情况
filter_corr.sort_values(ascending=0)  #按照相关行业间的相关系数进行排序
```

## 5. 策略结果及分析

相关性显著的行业如表 6-1 所示。

### 表 6-1

| 行业 1 | 行业 2 | corr |
|---|---|---|
| 传媒 | 轻工制造 | 0.527449 |
| 电子 | 家用电器 | 0.523264 |
| 国防军工 | 轻工制造 | 0.460587 |
| 国防军工 | 综合 | 0.436172 |
| 农林牧渔 | 国防军工 | 0.431786 |
| 电气设备 | 建筑材料 | -0.44004 |
| 商业贸易 | 食品饮料 | -0.46866 |
| 食品饮料 | 纺织服装 | -0.51695 |

如表 6-1 所示，我们可以得到所有统计显著的行业 1 与行业 2 一阶滞后的相关系数。从总体来看，相关系数比较大，行业相关性较强，但因为我们选取的是 2015 年 1 月之后的月度数据，数据样本量不够大，因此结果中的相关系数也较大。我们在进行实际操作的过程中可以根据自己的需求选择较长的数据区间进行筛选。虽然样本区间比较短，但我们可以将这个策略作为参考来理解行业轮动策略。

从表 6-1 中可以看到，传媒行业和轻工制造的一阶滞后的相关性最高，其次是电子行业和家用电器，它们的相关性都超过了 0.5。因此，如果传媒行业在这期涨得比较好，那么下一期我们应该超配轻工制造行业；如果电子行业在这期涨得比较好，那么下一期我们应该超配家用电器行业。另外，食品饮料和纺织服装的一阶滞后的相关性超过了 -0.5，有较强的负相关性。

## 6. 行业轮动策略小结

本节从行业收益率排名的相关性出发，试图找出具有预测意义的各个行业之间的关系。通过分析行业与行业的一阶滞后的相关性，我们得到 8 个 99%置信水平是统计显著的关系。其中传媒行业和轻工制造、电子行业和家用电器相关性最高均超过了 0.5，即这期传媒业的收益率排名与下一期轻工制造行业的收益率排名显著正相关；这期电子行业的收益率排名与下一期家用电器行业的收益率排名显著正相关。

## 6.3 市场中性 Alpha 策略

### 6.3.1 市场中性 Alpha 策略介绍

市场中性策略是指同时构建多头和空头头寸以对冲市场风险，无论市场处于上涨势还是下跌势的环境下，均能获得稳定收益的一种投资策略。也就是说，市场中性 Alpha 策略是一类收益与市场涨跌无关，致力于获取绝对收益的低风险量化投资策略，其主要通过同时持有股票多头和期货空头，来获取多头组合超越期货所对应的基准指数的收益。

### 6.3.2 市场中性 Alpha 策略的思想和方法

Alpha 策略最初是由 William Sharpe 在 1964 年的著作《投资组合理论与资本市场》中首次提出的，并指出投资者在市场中交易面临系统性风险和非系统性风险，用公式表达如下：

E(R_p) = R_f + β × (R_m - R_f)

其中，β = Cov(R_i, R_m)/Var(R_m)，E(R_p)表示投资组合的期望收益率，R_f 表示无风险报酬率，R_m 表示市场组合收益率，β 为某一组合的系统风险系数。CAPM 模型主要表示单个证券或投资组合同系统风险收益率之间的关系，也就是说，单个投资组合的收益率等于无风险收益率与风险溢价之和。

Alpha 策略的思想是通过衍生品来对冲投资组合的系统风险 β，锁定超额收益 Alpha。因此首先需要寻找稳定的 Alpha，构建 Alpha 组合，进而计算组合的 β 来对冲风险。Alpha 策略成功的关键就是寻找到一个超越基准（具有股指期货等做空工具的基准）的策略。比如，可以构造指数增强组合+沪深 300 指数期货空头策略。这种策略隐含的投资逻辑是择时比较困难，不想承受市场风险。

该策略的方法如图 6-2 所示。

![](img/0b89c6ed12810914fc2fb80a08af1fae_217_0.png)

图 6-2

市场中性 Alpha 策略是一套科学且成熟的投资研究方法，在实际应用中，我们通常采用多因子模型驱动、期货对冲来获取长期稳定的 Alpha。同时，使用多因子模型可有效地结合基本面和技术面，使策略更可靠、更稳健。另外，该策略的投资标的广、市场容量大，基本没有产品规模限制。

### 6.3.3 实例展示

通过上面的介绍，我们对市场中性 Alpha 策略有了较清晰的认识，这里利用多因子模型构建一个股票组合，同时选取基准指数对应的期货进行对冲，就可以构建一个完整的市场中性 Alpha 策略。

下面是一个具体的市场中性策略的实例。

- 回测区间：2015 年 1 月 1 日至 2015 年 8 月 26 日。
- 基准：沪深 300 指数。
- 股票池：沪深 300 动态成分股，在每个交易日都进行调仓。

对因子的选取和处理如下。

- 对因子的选取：净利润增长率、净资产收益率、相对强弱指数（RSI）。
- 对因子的处理：用到了去极值（Winsorize）、标准化（Standardize）、中性化（Neutralize）处理。
- 组合构建：等权配置。

空头期货对冲为：空头使用期货 IF 当月合约做空来进行对冲。如果距离合约到期日有 3 天，则移仓换月。IFL0 表示当月合约，IFL1 表示次月合约。

代码展示如下：

```
from CAL.PyCAL import *
import numpy as np
from pandas import DataFrame
start = '2015-01-01'                  # 回测起始时间
end = '2015-08-26'                    # 回测结束时间
benchmark = 'HS300'                  # 策略参考标准
universe = DynamicUniverse('HS300') + ['IFL0', 'IFL1']
# 证券池，支持股票和基金
capital_base = 10000000              # 起始资金
```

```
freq = 'd'
# 策略类型，'d'表示日间策略使用日线回测，'m'表示日内策略使用分钟线回测
refresh_rate = 1
cal = Calendar('China.SSE')
period = Period('-1B')

# 账户初始化配置
stock_commission = Commission(buycost=0.0, sellcost=0.0, unit='perValue')
futures_commission=Commission(buycost=0.0, sellcost=0.0, unit='perValue')
slippage = Slippage(value=0, unit='perValue')

accounts = {
    'stock_account':AccountConfig(account_type='security',capital_base=
capital_base, commission=stock_commission, slippage=slippage),
    'futures_account':AccountConfig(account_type='futures',capital_base=
capital_base, commission=futures_commission, slippage=slippage)
}

#策略算法
def initialize(context):
    context.signal_generator=SignalGenerator(Signal('NetProfitGrowRate'),
Signal('ROE'), Signal('RSI'))
    context.need_to_switch_position = False
    context.contract_holding = ''

def handle_data(context):
    # log.info(context.current_date)
    universe = context.get_universe(exclude_halt=True)

    yesterday = context.previous_date
    signal_composite = DataFrame()

    # 净利润增长率
    NetProfitGrowRate = context.signal_result['NetProfitGrowRate']
    signal_NetProfitGrowRate=standardize(neutralize(winsorize
(NetProfitGrowRate),yesterday.strftime('%Y%m%d')))
    signal_composite['NetProfitGrowRate'] = signal_NetProfitGrowRate

    # 权益收益率
    ROE = context.signal_result['ROE']
    signal_ROE=standardize(neutralize(winsorize(ROE),yesterday.strftime
('%Y%m%d')))
    signal_composite['ROE'] = signal_ROE
    # RSI
    RSI = context.signal_result['RSI']
    signal_RSI=standardize(neutralize(winsorize(RSI),yesterday.strftime('%Y%m%d')))
    signal_composite['RSI'] = signal_RSI
    # 信号合成，各因子权重
    weight = np.array([0.6, 0.3, 0.1])
    signal_composite['total_score'] = np.dot(signal_composite, weight)
    # 组合构建
    total_score = signal_composite['total_score'].to_dict()
    wts = simple_long_only(total_score, yesterday.strftime('%Y%m%d'))
    handle_stock_orders(context, wts)
    handle_futures_orders(context)
    # handle_futures_position_switch(context)
    # 订单委托
    def handle_stock_orders(context, wts):
    account = context.get_account('stock_account')
    # 先卖出
    sell_list = account.get_positions()
    for stk in sell_list:
        account.order_to(stk, 0)
    # 再买入
    buy_list = wts.keys()
    total_money = account.portfolio_value
    prices = account.reference_price
    for stk in buy_list:
        if np.isnan(prices[stk]) or prices[stk] == 0: # 停牌或者还没有上市等原因不能交易
            continue
        account.order(stk, int(total_money * wts[stk] / prices[stk]
/100)*100)
    def handle_futures_orders(context):
    stock_account = context.get_account('stock_account')
```
