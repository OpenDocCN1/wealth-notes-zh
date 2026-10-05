# 8.3 多曲线金融指标

常见的MACD指标及PivotPoint（支撑点）指标都是复杂的多曲线金融指标，包含多组指标曲线。

BackTrader支持复杂的多曲线金融指标曲线的绘制，而且绘制模式非常灵活。下面以PivotPoint指标为例进行详细讲解。

以下是与PivotPoint指标相关的代码：

```
class PivotPoint(bt.Indicator):
    lines = ('p', 's1', 's2', 'r1', 'r2',)
    plotinfo = dict(subplot=False)

    def __init__(self):
        h = self.data.high  # current high
        l = self.data.low  # current high
        c = self.data.close  # current high

        self.lines.p = p = (h + l + c) / 3.0

        p2 = p * 2.0
        self.lines.s1 = p2 - h  # (p x 2) - high
        self.lines.r1 = p2 - l  # (p x 2) - low

        hilo = h - l
        self.lines.s2 = p - hilo  # p - (high - low)
        self.lines.r2 = p + hilo  # p + (high - low)
```

PivotPoint指标包含多组曲线，如图8-6所示。