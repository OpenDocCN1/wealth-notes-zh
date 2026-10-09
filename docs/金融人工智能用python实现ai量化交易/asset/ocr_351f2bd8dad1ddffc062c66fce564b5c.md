对单一神经网络日内交易的预测结果准确率依然在 50% 左右宽幅波动。好的方面是，一些标的的准确率可以在 55% 以上。元模型通过装袋的方法表现出了在样本外更加一致的预测效果，尽管很多观测到的准确率只略高于基准值 50%。

```
In [56]: %time train_test_model(model_n_imp)
OUT-OF-SAMPLE | AAPL.O | acc=0.4520
OUT-OF-SAMPLE | MSFT.O | acc=0.4930
OUT-OF-SAMPLE | INTC.O | acc=0.5549
OUT-OF-SAMPLE | AMZN.O | acc=0.4709
OUT-OF-SAMPLE | GS.N | acc=0.5184
OUT-OF-SAMPLE | SPY | acc=0.4860
OUT-OF-SAMPLE | .SPX | acc=0.5019
OUT-OF-SAMPLE | .VIX | acc=0.4885
OUT-OF-SAMPLE | EUR= | acc=0.5150
OUT-OF-SAMPLE | XAU= | acc=0.4824
OUT-OF-SAMPLE | GD X | acc=0.4765
OUT-OF-SAMPLE | GLD | acc=0.5455
CPU times: user 1min 4s, sys: 5.05 s, total: 1min 9s
Wall time: 9.56 s
```

```
In [57]: %time train_test_model(model_bag)
OUT-OF-SAMPLE | AAPL.O | acc=0.5660
OUT-OF-SAMPLE | MSFT.O | acc=0.5431
OUT-OF-SAMPLE | INTC.O | acc=0.5072
OUT-OF-SAMPLE | AMZN.O | acc=0.5110
OUT-OF-SAMPLE | GS.N | acc=0.5020
OUT-OF-SAMPLE | SPY | acc=0.5120
OUT-OF-SAMPLE | .SPX | acc=0.4677
OUT-OF-SAMPLE | .VIX | acc=0.5092
OUT-OF-SAMPLE | EUR= | acc=0.5242
OUT-OF-SAMPLE | XAU= | acc=0.5255
OUT-OF-SAMPLE | GD X | acc=0.5085
OUT-OF-SAMPLE | GLD | acc=0.5374
CPU times: user 2.64 s, sys: 439 ms, total: 3.08 s
Wall time: 12.4 s
```

![](bbox=[0.17, 0.62, 0.25, 0.72])

# 日内交易的有效市场

尽管对日终交易来说市场是弱有效的，但是对日内交易而言，市场绝不是弱有效的。上述统计失败的结果可能是由于短暂性的不平衡、买入或卖出压力、市场过度反应、技术性的买卖指令触发等原因造成的。关键的问题是，统计失效一旦出现，能否通过特定的交易策略获得收益。

# 6.5 结论

Halevy 等（2009）在其撰写且被广泛引用的 “The Unreasonable Effectiveness of Data” 一文中指出，经济学家往往存在所谓的 “物理幻觉”。也就是说，经济学家十分羡慕物理学家可以通过优雅的数学推导来解释复杂的现实世界现象，而他们却没有办法用同样念方式来解释人类行为。最为人们熟知的类似例子是爱因斯坦的 $E = mc^{2}$ ，它将复杂的能量概念等同于质量与速度平方的乘积。