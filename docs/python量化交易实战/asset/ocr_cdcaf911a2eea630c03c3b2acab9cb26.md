求其稳定性，对其个股的成长性并没有太大要求。
因此，基于分析可以得出，对于中证红利，主要有以下几个方面要进行关注。

+   - 股息率：股息率（Dividend Yield Ratio）是一年的总派息额与当时市价的比例。以占股票最后销售价格的百分数表示年度股息，该指标是投资收益率的简化形式。股息率是股息与股票价格之间的比率。在投资实践中，股息率是衡量企业是否具有投资价值的重要标尺之一。

+   - 市净率：市净率指的是每股股价与每股净资产的比率。市净率可用于股票投资分析，一般来说市净率较低的股票，投资价值较高，相反则投资价值较低。但在判断投资价值时，还要考虑当时的市场环境以及公司经营情况、盈利能力等因素。

+   - 市盈率：市盈率（Price Earnings ratio，即 P/E ratio）也称“本益比”“股价收益比率”或“市价盈利比率”。

+   - 贝塔值：与大盘的联动波动率。

+   - 自相关波动率：自身的波动率。

基于以上分析，本节开始构建基于中证红利的指数增强基金，综合运用以上指标，搭建一个能够获取高收益、高稳态、低风险的收益组合。

# 8.2.1 中证红利策略的构建方法

通过前面的学习可以知道，对于组合的构建最基本的就是找到若干只符合要求的股票，组合成一个股票池，这个股票池反映的是构建者的认知与能力。

# 基本面因子的获取

基本面因子是中证红利构成因子的基础，在这里主要选用市净率、市盈率以及股息率作为基本的选股要求，代码如下：

```
df = get_fundamentals(table='trading_derivative_indicator',
 symbols=symbol_list,start_date=last_day,end_date=last_day,fields='PETTM,DY,PB',
 df=True)
```

除此之外，还需要添加波动率因子，前面在介绍技术因子时对其也有涉及，这里根据需要采用了两个技术因子：Beta 值与自相关波动率，代码如下：

```
#这里使用收益进行计算
def get_beta_weight_2(symbol,now,count,market_index = "SHSE.000001"):
    """
一般用单个股票资产的历史收益率对同期指数（大盘）收益率进行回归，回归系数就是 Beta 系数。
"""
last_day = get_previous_trading_date("SHSE",now)
market_data = history_n(symbol=market_index, frequency="1d", count=count,
    end_time=last_day,
    fields="high,low,close",
```