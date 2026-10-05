```
df_factor_3['NetProfitGrowRate'] = np.where(df_factor_3['NetProfitGrowRate'] > 0, 1, 0)
    df_factor_3.rename(columns={'NetProfitGrowRate': 'N3'}, inplace=True)
    df_factor = df_factor.merge(df_factor_3, on='secID')

    df_factor_4 = DataAPI.MktStockFactorsOneDayGet(secID=universe, tradeDate=trade_date,field=['secID','NetProfitGrowRate'],pandas="1")
    df_factor_4['NetProfitGrowRate'] = np.where(df_factor_4['NetProfitGrowRate'] > 0, 1, 0)
    df_factor_4.rename(columns={'NetProfitGrowRate': 'N4'}, inplace=True)
    df_factor = df_factor.merge(df_factor_4, on='secID')
    df_factor['N_all'] = df_factor['N'] + df_factor['N1'] + df_factor['N2'] + df_factor['N3'] + df_factor['N4']

    # 获取股票型基金列表
    tmp_fundlist = fundlist[fundlist['establishDate'] <= trade_date]
    # 获取报告期
    if trade_date[5:7] == '04':
        reportDate = '{}-12-31'.format(str(int(trade_date[:4]) - 1))
    elif trade_date[5:7] == '08':
        reportDate = '{}-06-30'.format(trade_date[:4])
    else:
        reportDate = '{}-09-30'.format(trade_date[:4])
    fund_hold = DataAPI.FundHoldingsGet(reportDate=reportDate,secID=u"", ticker=tmp_fundlist.ticker,secType="E",field=u"ticker,holdingTicker",pandas="1")
    fund_hold = fund_hold.groupby('holdingTicker').count()
    fund_hold.reset_index(inplace=True)

    # 获取总持仓基金个数
    fund_hold.rename(columns={'holdingTicker':'ticker','ticker':'holdnumber'}, inplace=True)
    df_factor = df_factor.merge(fund_hold, on='ticker')

    # 获取总市值及收盘价
    df_factor1=DataAPI.MktEqudGet(secID=universe,tradeDate=trade_date, field=u"secID,marketValue,closePrice",pandas="1")
    # 计算总股本
    df_factor1['totalshare']=df_factor1['marketValue']/df_factor1['closePrice']
```