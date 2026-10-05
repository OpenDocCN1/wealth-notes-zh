```
#--------------------------------------------------------------------------------
def sendOrder(self, vtSymbol, direction, offset, price, volume, multiplier):
    """"""
    # 计算合约持仓
    if direction == DIRECTION_LONG:
        self.unitDict[vtSymbol] += volume        # volume 即为单位头寸
        self.posDict[vtSymbol] += volume * multiplier
    else:
        self.unitDict[vtSymbol] -= volume
        self.posDict[vtSymbol] -= volume * multiplier  # 实际持仓=单位头寸 × 头寸
#规模
```

# 2. 单位头寸限制

基于高度关联市场和松散关联市场判断起来都非常主观，并无统一标准。客观上只能实现单个市场和单个方向的头寸限制。

在开仓交易前，需要检查上一笔交易是否盈利，若盈利则直接返回，不进行买卖操作（仅仅适用于短周期版本的入场策略，即 profitCheck=True；长周期版本对应的是 profitCheck=False）。

```
MAX_PRODUCT_POS = 4        # 单品种最大持仓
MAX_DIRECTION_POS = 10     # 单方向最大持仓
.....
class TurtlePortfolio(object):
.....
    #--------------------------------------------------------------------------------
    def newSignal(self, signal, direction, offset, price, volume):
.....
        # 开仓
        if offset == OFFSET_OPEN:
            # 检查上一次是否为赢利
            if signal.profitCheck:
                pnl = signal.getLastPnl()
                if pnl > 0:
                    return
            # 买入
            if direction == DIRECTION_LONG:
                # 组合持仓不能超过上限
                if self.totalLong >= MAX_DIRECTION_POS:
```