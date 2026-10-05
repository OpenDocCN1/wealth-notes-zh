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

# 使用通联 CAL 包中的 BS呍pliedVolatity 计算隐含波动率
calculated_vol = BSImpliledVolatity(opt_types, strike, spot, risk_free, 0.0, time_to_maturity, settle)
calculated_vol = calculated_vol.fillna(0.0)

# 使用通联 CAL 包中的 BSMPrice 计算期权风险指标
greeks = BSMPrice(opt_types, strike, spot, risk_free, 0.0, calculated_vol.vol.values, time_to_maturity)
greeks.vega = greeks.vega #/ 100.0
```