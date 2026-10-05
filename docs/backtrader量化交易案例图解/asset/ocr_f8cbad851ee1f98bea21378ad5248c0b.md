```
cerebro.addstrategy(TQSta001,maperiod=5,fgPrint=False)
```

此外，案例运行后，在输出信息中按日期生成的交易订单信息的最后有一条信息：

```
2018-12-28, (MA均线周期变量Period= 15), 最终资产总值: 100007.05
```

这个是策略类stop函数，在策略回测完成后生成的输出信息中，有当前策略的最终资产总值和当前策略所有的变量参数。

```
def stop(self):
    # 新增一个stop 策略完成函数
    # 用于输出执行后的数据
    self.log('(MA均线周期变量Period= %2d), 最终资产总值: %.2f' %
             (self.params.mapperiod, self.broker.getvalue()), fgPrint=True)
```

如图12-4所示是BackTrader量化软件中策略类stop函数关系属性示意图。