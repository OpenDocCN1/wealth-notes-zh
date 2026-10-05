
## 6.11 期权策略：Greeks 和隐含波动率微笑计算

在本文中，我们将通过通联数据提供的数据进行上证 50ETF 期权的隐含波动率微笑计算。

### 6.11.1 数据准备

将上海银行间同业拆借利率 SHIBOR 作为无风险利率参考：

```
from CAL.PyCAL import *
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib import rc
rc('mathtext', default='regular')
import seaborn as sns
sns.set_style('white')
import math
from scipy import interpolate
from scipy.stats import mstats
from pandas import Series, DataFrame, concat
import time
from matplotlib import dates
## 银行间质押式回购利率
def getHistDayInterestRateInterbankRepo(date):
    cal = Calendar('China.SSE')
    period = Period('-10B')
    begin = cal.advanceDate(date, period)
    begin_str = begin.toISO().replace('-', '')
    date_str = date.toISO().replace('-', '')
    # 以下 indicID 对应的银行间质押式回购利率周期分别为:
    # 1D、7D、14D、21D、1M、3M、4M、6M、9M、1Y
    indicID = [u"1090000566", u"1090000567", u"1090000568", u"1090000569", u"1090000570", u"1090000571", u"1090000572", u"1090000573", u"1090000574", u"1090000575"]
    period = np.asarray([1.0, 7.0, 14.0, 21.0, 30.0, 90.0, 120.0, 180.0, 270.0,
```

```
360.0]) / 360.0
    period_matrix = pd.DataFrame(index=indicID, data=period, columns=['period'])
    field = [u"indicID",u"publishDate",u"periodDate",u"dataValue"]
    interbank_repo = DataAPI.EcoDataProGet(indicID, begin_str, date_str)
    interbank_repo = interbank_repo.groupby('indicID').first()
    interbank_repo.index = [str(i) for i in interbank_repo.index]
    interbank_repo = concat([interbank_repo, period_matrix], axis=1, join='inner').sort_index()
    return interbank_repo

## 银行间同业拆借利率
def getHistDaySHIBOR(date):
    date_str = date.toISO().replace('-', '')
    # 以下 indicID 对应的 SHIBOR 周期分别为：
    # 1D、7D、14D、1M、3M、6M、9M、1Y
    indicID = [u'1090000537',u'1090000538',u'1090000539',u'1090000540',
               u'1090000541',u'1090000542',u'1090000543',u'1090000544']
    period = np.asarray([1.0, 7.0, 14.0, 30.0, 90.0, 180.0, 270.0, 360.0]) / 360.0
    period_matrix = pd.DataFrame(index=indicID, data=period, columns=['period'])
    field = [u"indicID",u"publishDate",u"periodDate",u"dataValue"]
    interest_shibor = DataAPI.EcoDataProGet(indicID, beginDate=date_str, endDate=date_str)[field]
    interest_shibor = interest_shibor.set_index('indicID')
    interest_shibor.index = [str(i) for i in interest_shibor.index]
    interest_shibor = concat([interest_shibor, period_matrix], axis=1, join='inner').sort_index()
    return interest_shibor

## 插值得到给定的周期的无风险利率
def periodsSplineRiskFreeInterestRate(date, periods):
    # 此处使用SHIBOR 来插值
    init_shibor = getHistDaySHIBOR(date)
    shibor = {}
    min_period = min(init_shibor.period.values)
    min_period = 25.0/360.0
    max_period = max(init_shibor.period.values)
    for p in periods.keys():
```

```
tmp = periods[p]
if periods[p] > max_period:
    tmp = max_period * 0.99999
elif periods[p] < min_period:
    tmp = min_period * 1.00001
sh = interpolate.spline(init_shibor.period.values, init_shibor.dataValue.values, [tmp], order=3)
    shibor[p] = sh[0]/100.0
return shibor
```

### 6.11.2 Greeks 和隐含波动率计算

这里计算如下 Greeks。

- delta：期权价格关于标的价格的一阶导数。
- gamma：期权价格关于标的价格的二阶导数。
- rho：期权价格关于无风险利率的一阶导数。
- theta：期权价格关于到期时间的一阶导数。
- vega：期权价格关于波动率的一阶导数。

有以下注意事项。

- 在计算隐含波动率时采用 Black-Scholes-Merton 模型，此模型在平台 Python 包的 CAL 中已有实现。
- 对无风险利率使用 SHIBOR。
- 在期权的时间价值为负时（此种情况在 50ETF 期权里时有发生），无法通过 BSM 模型计算隐含波动率，所以此时将期权隐含波动率设为 0.0，实际上，此时的隐含波动率和各风险指标并无实际参考价值。

代码如下：

```
## 使用 DataAPI.OptGet、DataAPI.MktOptdGet 拿到计算所需的数据
def getOptDayData(opt_var_sec, date):
    date_str = date.toISO().replace('-', '')

#使用 DataAPI.OptGet 拿到已退市和上市的所有期权的基本信息
```

```
info_fields = [u'optID', u'varSecID', u'varShortName', u'varTicker', u'varExchangeCD', u'varType', u'contractType', u'strikePrice', u'contMultNum', u'contractStatus', u'listDate', u'expYear', u'expMonth', u'expDate', u'lastTradeDate', u'exerDate', u'deliDate', u'delistDate']

opt_info = DataAPI.OptGet(optID='', contractStatus=[u"DE",u"L"], field=info_fields, pandas="1")

#使用 DataAPI.MktOptdGet 拿到历史上某一天的期权成交信息
mkt_fields = [u'ticker', u'optID', u'secShortName', u'exchangeCD', u'tradeDate', u'preSettlePrice', u'preClosePrice', u'openPrice', u'highestPrice', u'lowestPrice', u'closePrice', u'settlPrice', u'turnoverVol', u'turnoverValue', u'openInt']

opt_mkt = DataAPI.MktOptdGet(tradeDate=date_str, field=mkt_fields, pandas = "1")

opt_info = opt_info.set_index(u"optID")
opt_mkt = opt_mkt.set_index(u"optID")
opt = concat([opt_info, opt_mkt], axis=1, join='inner').sort_index()
return opt

## 分析历史某一日的期权收盘价信息，得到隐含波动率微笑和期权风险指标
def getOptDayAnalysis(opt_var_sec, date):
    opt = getOptDayData(opt_var_sec, date)

    #使用 DataAPI.MktFunddGet 拿到期权标的的日行情
    date_str = date.toISO().replace('-', '')
    opt_var_mkt = DataAPI.MktFunddGet(secID=opt_var_sec,tradeDate=date_str, beginDate=u"",endDate=u"",field=u"",pandas="1")
    #opt_var_mkt = DataAPI.MktFunddAdjGet(secID=opt_var_sec,beginDate=date_str,endDate=date_str,field=u"",pandas="1")

    # 计算 shibor
    exp_dates_str = opt.expDate.unique()
    periods = {}
    for date_str in exp_dates_str:
        exp_date = Date.parseISO(date_str)
        periods[exp_date] = (exp_date - date)/360.0
    shibor = periodsSplineRiskFreeInterestRate(date, periods)
```

```
settle = opt.settlPrice.values          # 期权 settle price
close = opt.closePrice.values          # 期权 close price
strike = opt.strikePrice.values        # 期权 strike price
option_type = opt.contractType.values  # 期权类型
exp_date_str = opt.expDate.values      # 期权行权日期
eval_date_str = opt.tradeDate.values   # 期权交易日期

mat_dates = []
eval_dates = []
spot = []
for epd, evd in zip(exp_date_str, eval_date_str):
    mat_dates.append(Date.parseISO(epd))
    eval_dates.append(Date.parseISO(evd))
    spot.append(opt_var_mkt.closePrice[0])
time_to_maturity = [float(mat - eva + 1.0)/365.0 for (mat, eva) in zip(mat_dates, eval_dates)]

risk_free = []  # 无风险利率
for s, mat, time in zip(spot, mat_dates, time_to_maturity):
    #rf = math.log(forward_price[mat] / s) / time
    rf = shibor[mat]
    risk_free.append(rf)

opt_types = []   # 期权类型
for t in option_type:
    if t == 'CO':
        opt_types.append(1)
    else:
        opt_types.append(-1)

# 使用通联 CAL 包中的 BSImpliedVolatility 计算隐含波动率
calculated_vol = BSImpliedVolatility(opt_types, strike, spot, risk_free, 0.0, time_to_maturity, settle)
calculated_vol = calculated_vol.fillna(0.0)

# 使用通联 CAL 包中的 BSMPrice 计算期权风险指标
greeks = BSMPrice(opt_types, strike, spot, risk_free, 0.0, calculated_vol.vol.values, time_to_maturity)
greeks.vega = greeks.vega #/ 100.0
```

```
greeks.rho = greeks.rho #/ 100.0
greeks.theta = greeks.theta #* 365.0 / 252.0 #/ 365.0

opt['strike'] = strike
opt['optType'] = option_type
opt['expDate'] = exp_date_str
opt['spotPrice'] = spot
opt['riskFree'] = risk_free
opt['timeToMaturity'] = np.around(time_to_maturity, decimals=4)
opt['settle'] = np.around(greeks.price.values.astype(np.double), decimals=4)
opt['iv'] = np.around(calculated_vol.vol.values.astype(np.double), decimals=4)
opt['delta'] = np.around(greeks.delta.values.astype(np.double), decimals=4)
opt['vega'] = np.around(greeks.vega.values.astype(np.double), decimals=4)
opt['gamma'] = np.around(greeks.gamma.values.astype(np.double), decimals=4)
opt['theta'] = np.around(greeks.theta.values.astype(np.double), decimals=4)
opt['rho'] = np.around(greeks.rho.values.astype(np.double), decimals=4)

fields = [u'ticker', u'contractType', u'strikePrice', u'expDate', u'tradeDate', u'closePrice', u'settlPrice', 'spotPrice', u'iv', u'delta', u'vega', u'gamma', u'theta', u'rho']
opt = opt[fields].reset_index().set_index('ticker').sort_index()
#opt['iv'] = opt.iv.replace(to_replace=0.0, value=np.nan)
return opt
```

尝试用 getOptDayAnalysis 计算 2015 年 9 月 24 日这一天的风险指标：

```
# Uqer 计算期权的风险数据
opt_var_sec = u"510050.XSHG"   # 期权标的
date = Date(2015, 9, 24)

option_risk = getOptDayAnalysis(opt_var_sec, date)
option_risk.head(2)
```

|   | optID | contractType | strikePrice | expDate | tradeDate | closePrice | settlPrice | spotPrice | iv | delta | vega | gamma | theta | rho |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| ticker |   |   |   |   |   |   |   |   |   |   |   |   |   |   |
| 510050C1510N01850 | 10008405 | CO | 1.85 | 2015-10-28 | 2015-09-24 | 0.3268 | 0.3555 | 2.187 | 0.4317 | 0.9101 | 0.1099 | 0.5550 | -0.2992 | 0.1568 |
| 510050C1510N01900 | 10008406 | CO | 1.90 | 2015-10-28 | 2015-09-24 | 0.2791 | 0.3102 | 2.187 | 0.4161 | 0.8810 | 0.1347 | 0.7058 | -0.3435 | 0.1550 |

getOptDayAnalysis 函数计算结果如下：

```
# 本文计算结果 option_risk

near_exp = np.sort(option_risk.expDate.unique())[0]    # 近月期权行权日

opt_call_uqer = option_risk[option_risk.expDate==near_exp][option_risk.contractType=='CO'].set_index('strikePrice')

opt_put_uqer = option_risk[option_risk.expDate==near_exp][option_risk.contractType=='PO'].set_index('strikePrice')

## -------------------------------
## 风险指标
fig = plt.figure(figsize=(10,12))
fig.set_tight_layout(True)

# ------ Delta ------
ax = fig.add_subplot(321)
ax.plot(opt_call_uqer.index, opt_call_uqer['delta'], '-o')
ax.plot(opt_put_uqer.index, opt_put_uqer['delta'], '-o')
ax.legend(['call-uqer', 'put-uqer'])
ax.grid()
ax.set_xlabel(u"strikePrice")
ax.set_ylabel(r"Delta")
plt.title('Delta Comparison')

# ------ Theta ------
ax = fig.add_subplot(322)
ax.plot(opt_call_uqer.index, opt_call_uqer['theta'], '-o')
ax.plot(opt_put_uqer.index, opt_put_uqer['theta'], '-o')
ax.legend(['call-uqer', 'put-uqer'])
ax.grid()
ax.set_xlabel(u"strikePrice")
ax.set_ylabel(r"Theta")
plt.title('Theta Comparison')
```
