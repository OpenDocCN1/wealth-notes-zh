```
print('\t 利润总额: %.2f,' % dget)
print('\t ROI 投资回报率 Return on investment: %.2f %%' % kret)

#
#--------
print('\n#8,Analyzer 分析 BT 量化回测数据')
strat =results[0]
anzs=strat.analyzers
#
dsharp=anzs.SharpeRatio.get_analysis()['sharperatio']
trade_info=anzs.TradeAnalyzer.get_analysis()
#
dw=anzs.DW.get_analysis()
max_drawdown_len =dw['max']['len']
max_drawdown =dw['max']['drawdown']
max_drawdown_money =dw['max']['moneydown']
#
print('\t 夏普指数 SharpeRatio : ',dsharp)
print('\t 最大回撤周期 max_drawdown_len : ', max_drawdown_len)
print('\t 最大回撤 max_drawdown : ', max_drawdown)
print('\t 最大回撤(资金)max_drawdown_money : ', max_drawdown_money)

#--------
#
print('\n#9,绘制 BT 量化分析图形')
cerebro.plot()

#--------
#
print('\nzook')
```