```
{
  "fontFamily": "微软雅黑",
  "fontSize": 12,
  "mongoHost": "localhost",
  "mongoPort": 27017,
  "mongoLogging": false,
  "darkStyle": false,
  "language": "chinese",
  "logActive": true,
  "logLevel": "debug",
  "logConsole": false,
  "logFile": false,
  "tdPenalty": ["IFM", "THF", "TCH"],
  "maxDecimal": ...
}
```

图 7-9 设置平今惩罚模式

下面展示底层委托转换函数。

```
def convertOrderReq(self, req):
    """转换委托请求"""
    # 普通模式无须转换
    if self.mode is self.MODE_NORMAL:
        return [req]

    # 上期所模式拆分今昨, 优先平今
    elif self.mode is self.MODE_SHFE:
        # 开仓无须转换
        if req.offset is OFFSET_OPEN:
            return [req]

        # 多头
        if req.direction is DIRECTION_LONG:
            posAvailable = self.shortPos - self.shortPosFrozen
            tdAvailable = self.shortTd - self.shortTdFrozen
            ydAvailable = self.shortYd - self.shortYdFrozen
        # 空头
        else:
            posAvailable = self.longPos - self.longPosFrozen
            tdAvailable = self.longTd - self.longTdFrozen
            ydAvailable = self.longYd - self.longYdFrozen

        # 平仓量超过总可用, 拒绝, 返回空列表
```