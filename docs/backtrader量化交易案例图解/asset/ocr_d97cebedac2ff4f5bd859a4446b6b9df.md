```
print('\n\t#2-5,设置 addanalyzer 分析参数')
#
#不同时间周期
cerebro.addanalyzer(bt.analyzers.TimeReturn, timeframe=bt.TimeFrame.Years,_name='timReturns')
#cerebro.addanalyzer(bt.analyzers.TimeReturn, timeframe=bt.TimeFrame.Months,_name='timReturns')
#
cerbero.addanalyzer(bt.analyzers.AnnualReturn, _name='AnnualReturn')
#
cerbero.addanalyzer(SQN,_name='sqnAnz')
#
cerbero.addanalyzer(bt.analyzers.SharpeRatio, _name = 'SharpeRatio', legacyannual=True)
# 动态加权回报率 Variability-Weighted Return: Better SharpeRatio with Log Returns
cerebro.addanalyzer(bt.analyzers.VWR, _name='VWR')
#
cerbero.addanalyzer(bt.analyzers.DrawDown, _name='DW')
```

注意，在案例中TimeReturn（时间周期回报率）相关代码使用的是year（年）周期参数，也可以 некоторымmonth（月）、week（周）或者day（日）等。

Python多功能函数的接口不同、功能不同，系统内部会自动识别，这也说明Python语言非常灵活。对于这种情况其他编程语言往往需要编写不同的函数代码，每个函数API接口也完全不同，非常复杂，不如