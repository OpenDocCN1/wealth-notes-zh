# 动态买卖气指标

技术因子-超买超卖型

```
DTM = IF(OPEN<OPEN[1],0,MAX((HIGH-OPEN),(OPEN-OPEN[1])))
DBM = IF(OPEN>=OPEN[1],0,MAX ((OPEN-LOW), OPEN-OPEN[1]))
STM = SUM(DTM,N)
SBM = SUM(DBM,N)
ADTM = IF(STM>SBM,(STM-SBM)/STM,IF(STM=SBM,0,(STM-SBM)/SBM))
ADTMMA = SMA(ADTM,M)
默认：N = 23、M = 8
```

# 说明

该指标用开盘价的向上波动幅度和向下波动幅度的距离差值来描述人气高低；ADTM指标在+1到-1之间波动，低于-0.5时为低风险区，高于+0.5时为高风险区；ADTM上穿ADTMMA时，买入股票；ADTM跌穿ADTMMA时，卖出股票。

> 【参考文献】
常规通用技术指标。