```
print('\n\t#2-5,设置 addanalyzer 分析参数')

cerebro.addanalyzer(SQN)
#
cerebro.addanalyzer(bt.analyzers.SharpeRatio, _name = 'SharpeRatio', legacyannual=True)
cerebro.addanalyzer(bt.analyzers.AnnualReturn, _name='AnnualReturn')
#
cerebro.addanalyzer(bt.analyzers.TradeAnalyzer, _name='TradeAnalyzer')
cerebro.addanalyzer(bt.analyzers.DrawDown, _name='DW')
```

以上代码为量化程序添加SQN指数、Sharp指数、MaxDown等常用量化分析指标。

addanalyzer策略添加函数中的name参数变量，主要用于分析结果和图表绘制。这些指标名称可以通过name参数设置，一般使用默认模式，即指标函数名称。

如图9-2所示是addanalyzer策略添加函数关系属性示意图。

![](bbox=[0.12, 0.55, 0.88, 0.88])