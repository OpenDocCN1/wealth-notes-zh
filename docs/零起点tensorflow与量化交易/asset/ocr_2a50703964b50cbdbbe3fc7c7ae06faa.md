# 9.3 交易接口函数

具体的交易接口函数和买卖操作，主要通过调用 sta_buy 和 sta_sell 函数完成的，这两个函数位于 ztools_sto 策略模块中。

+   sta_buy：买入策略函数。
	sa_sell：卖出策略函数。

所有的策略决定买卖时，都是通过这两个函数统一进行的，这样设计是为了统一交易接口，便于用户修改和扩展。

以下是在 avg 均值策略函数中，对买卖函数的具体调用。

```
if fgSell:stknun=sta_sell(qx)
if fgBuy and(stknun==0):stknun=sta_buy(qx)
```

以上是惯用的卖出优先模式，这也是现金为主的指导思想在量化分析中的具体应用。

系统支持空头交易，由变量 qx.trd_nilFlag 赋值控制。

+   qx.trd_nilFlag=True，即当空头标志变量为 True（真）时，支持空头交易。
	sx.trd_nilFlag=False，即当空头标志变量为 False（假）时，取消空头交易，默认不支持空头交易。

当 qx.trd-nilFlag=True 时，还有两个空头辅助变量将起作用。

+   qx.usrLevel：空头杠杆指数变量。按用户起点金额（变量为 qx.trd_buyMoney0）的倍数，提供空头上限金额支持。空头上限金额保存在变量 qx.usrMoneyOnil 中。

```
qx.usrMoneyOnil=qx.usrMoney0*qx.usrLevel
```

+   qx.usrMoneyOnil：空头上限金额变量。只有当空头标志变量 qx.trd_nilFlag 为 True 时，qx.usrMoneyOnil 和 qx.usrLevel 变量才起作用。

# 案例 9-3：回测分析

案例 9-3 文件名为 ke903_bt01.py，主要介绍回测分析的具体内容。下面我们对组进行讲解。