注意，在案例程序源码主代码最后的plot函数代码部分有一个style参数：

```
cerebro.plot()                          #默认是线条图或者上一次设置参数
#c erebro.plot(style='line')              #线条图
#c erebro.plot(style='candle')            #蜡烛图
#c erebro.plot(style='bar')               #曲线图，美式K线图
#c erebro.plot(style='ohlc')              #曲线图，美式K线图
```

BackTrader内置的plot绘图函数，通过style参数变量控制不同风格的K线图，默认有以下几种风格。

+   - line: 线条图。
- candle: 蜡烛图，标准K线图。
- ohlc: 美式K线图，和bar参数一样。
- bar: ohlc曲线图，美式K线图，和ohlc参数一样。

将style参数设置为bar或者ohlc，生成的图都是一样的。调整不同的style参数，运行程序后生成的相关图如图8-2～图8-4所示。