```
longEntered = False
shortEntered = False
```

# 3. onBar 函数

因为 Dual Thrust 策略是基于在通道突破的瞬间买入卖出的，即在 1 分钟 K 线内价格突破通道时触发条件进行交易，为了在追涨杀跌的过程中获得一个好的价格，所以在该 1 分钟内不保证成交。为了防止之前下的单子在上一分钟没有成交，但是下一分钟可能已经调整了价格，就用 cancelAll()方法立刻撤销之前未成交的所有委托，保证策略在当前这 1 分钟开始时的整个状态是清晰和唯一的。

由于策略是由昨天和当日这两天的 K 线数据来产生交易信号的，barList 的长度设置为 2。用 append(bar)把每一天 K 线数据插入到 barList 列表中，若该列表的长度小于或等于 2，会判断出数据缓存数量不足，则直接返回，不执行下面的任何操作了。当第 3 天到来时，barList 列表长度为 3，if 判断为 False，则会用 pop(0) 的方法来删除最老的数据，把最新的数据缓存下来。

```
def onBar(self, bar):
    """收到 Bar 推送（必须由用户继承实现）"""
    # 撤销之前发出的尚未成交的委托（包括限价单和停止单）
    self.cancelAll()

    # 计算指标数值
    self.barList.append(bar)

    if len(self.barList) <= 2:
        return
    else:
        self.barList.pop(0)
    lastBar = self.barList[-2]
```

当新的一天来临时，先对 dayHigh 做一个过滤，一般情况下第 1 天 dayHigh 为 0，从第 2 天起 dayHigh 不等于 0，有了昨天和今天的数据后，策略才可以真正运行，然后分别计算：

+   ○ 前 1 天的波动 range。