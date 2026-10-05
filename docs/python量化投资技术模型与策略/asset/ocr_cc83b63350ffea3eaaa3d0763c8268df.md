其中，a为平滑指数，一般取作2/(N+1)。

将这个公式展开之后，我们可以发现，过去的收盘价会随着时间的推移，其权重呈指数级减少。越靠近现在，其权重越大，这样就可以凸显近期收盘价的重要性。

利用EMA，我们可以计算MACD指标。

MACD全称为Moving Average Convergence Divergence，即指数平滑异动均线。MACD是《系统与预测》（System and Forecasts）的出版人杰拉尔德·阿佩尔发明的。

MACD由三根线组成，分别是DIFF线、DEA线和MACD线。其包含三个参数SHORT、LONG、M，分别代表不同天数的EMA，具体计算方法分为如下四步（假设使用标准的12天、26天和9天）。

+   1）计算两根EMA线，分别是EMA（12）和EMA（26）。

+   2）计算离差值，算式如下：

```
DIF = EMA ( 12 ) - EMA ( 26 )
```

+   3）计算DIF的9日EMA，即DEA，算式如下：