```
if req.volume > posAvailable:
    return []
# 平仓量小于今可用, 全部平今仓
elif req.volume <= tdAvailable:
    req.offset = OFFSET_CLOSETODAY
    return [req]
# 平仓量大于今可用, 先平今仓再平昨仓
else:
    l = []
    if tdAvailable > 0:
        reqTd = copy(req)
        reqTd.offset = OFFSET_CLOSETODAY
        reqTd.volume = tdAvailable
        l.append(reqTd)
    reqYd = copy(req)
    reqYd.offset = OFFSET_CLOSEYESTERDAY
    reqYd.volume = req.volume - tdAvailable
    l.append(reqYd)
    return l

# 平今惩罚模式, 没有今仓则平昨仓, 否则锁仓
elif self.mode is self.MODE_TDPENALTY:
    # 多头
    if req.direction is DIRECTION_LONG:
        td = self.shortTd
        ydAvailable = self.shortYd - self.shortYdFrozen
    # 空头
    else:
        td = self.longTd
        ydAvailable = self.longYd - self.longYdFrozen

# 这里针对开仓和平仓委托使用同一套逻辑

# 如果有今仓, 则只能开仓 (或锁仓)
if td:
    req.offset = OFFSET_OPEN
    return [req]
# 如果平仓量小于昨可用量, 全部平昨仓
elif req.volume <= ydAvailable:
```