# 钱德动量摆动指标

技术因子-动量反转型

```
CZ1 = IF(CLOSE-CLOSE[1]>0,CLOSE-CLOSE[1],0)
CZ2 = IF(CLOSE-CLOSE[1]<0,ABS(CLOSE-CLOSE[1]),0)
SU(N) = SUM(CZ1,N)
SD(N) = SUM(CZ2,N)
CMO = (SU(N)-SD(N))/(SU(N)+SD(N))*100
默认:N = 20
```

# 说明

CMO指标通过计算今收与昨收的价差来判断趋势的强弱，当CMO大于50时，处于超买状态；当CMO小于50时处于超卖状态。

【参考文献】
常规通用技术指标。