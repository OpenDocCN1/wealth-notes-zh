位、佣金等数据，具体代码如下：

```
css=',成交价: {:.2f},小计 Cost: {:.2f},佣金 Comm %:{:.2f}'.format(
    rder.executed.price,order.executed.value,order.executed.comm)
```

需要注意的是，首先，在main主流程中信号交易策略模式需要使用add_signal函数添加交易信号函数：

```
print('\n\t#2-3,添加BT量化回测程序对应的买卖点 signal 信号参数')
print('\t# 案例中使用 MA 均线与当前价格的差值作为买卖点信号')
print('\t# 买入 long，卖出 short 信号，都采用这个')
#
cerebro.add_signal(bt.SIGNAL_LONGSHORT, SMA_Signal,period=15)
```

然后，使用signal_strategy信号交易策略函数添加自定义的交易信号策略：

```
print('\n\t# 增加一个自定义信号策略')
print('\t# 用于输出买卖执行信息')
cerebro.signal_strategy(sgn_st001)
```

在传统策略交易模式中的main主流程中，只有一个策略添加函数addstrategy，并且还包含了策略的执行，以及订单数据的管理信息。

在main主流程中，需要添加两组函数：

+   - add_signal， 添加交易信号。
- signal_strategy， 添加信号策略。

下面我们看看买卖点的时间数据。

第一行输出信息就是买单执行信息：

```
2018-10-29 00:00:00,@ 买单执行 BUY EXECUTED,成交价: 6.00,小计 Cost: 60.00,佣金 Comm %0.06
```

上述输出信息只有订单执行的数据，没有订单设置的数据，这也是一个小bug，大家可以在策略函数中进一步细化，增加订单设置的数据。