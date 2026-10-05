MaxDown、交易数据等，代码如下：

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

第三步调用回测程序，开始run运行回测分析：

```
#
print('\n#3,调用 BT 回测入口程序, 开始执行 run 量化回测运算')
results=cerebro.run()
```

在run量化回测运行完成后，进入第四步，即回测数据分析，包括绘制量化分析图表。

在分析回测数据之前，先看一看案例的输出信息：

```
#4,完成 BT 量化回测运算
    起始资金 Starting Portfolio Value: 100000.00
    资产总值 Final Portfolio Value: 100007.05
    利润总额: 7.05,
    ROI 投资回报率 Return on investment: 0.01 %
```

以上输出信息，实际上没有调用分析模块库，就是一些简单的分析数据，但调用了两个函数获取量化现金、资产数额。

通过调用get函数，可以获取最终资产总值：