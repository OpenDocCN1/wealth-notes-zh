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