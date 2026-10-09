# 第10章 Tick数据回溯分析

图中qx.trdLib是交易记录表，它共有4个字段。

+   • time——时间。

• ID——用户交易订单编码，按日期回组。

• cash——当天的现金总额，已扣除当天的交易金额。

• usrPools——用户交易股票池数据。

最下面的userPools和trdLib是用户交易股票池数据，采用Python字典模式，每只参与交易的股票数据为一组，其中：

+   • 字典key值——股票代码。

• num9——持有该股票的总数额。

• dnum——当天的交易数额，正数为买入，负数为卖出。

第9.2组代码如下：

```
print('\n*9.2 tq_usrStkMerge')

df_usr=ztq.tq_usrStkMerge(qx)

zt.prDF('df_usr',df_usr)
```

合并用户交易数据，如图10-11所示是合并后的交易数据（部分）。

![](bbox=[0.2, 0.53, 0.7, 0.77])

其中相关的字段有：

+   • date——时间。