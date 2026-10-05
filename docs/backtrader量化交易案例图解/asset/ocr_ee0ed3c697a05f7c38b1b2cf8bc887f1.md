内部指标分布图。

![](bbox=[0.11, 0.13, 0.885, 0.475])

图13-2 Indicator子模块的内部指标分布图

由图13-2可以看出，BackTrader量化软件内置的金融指标大部分直接集成在Indicator子模块源码中。也有一些金融指标采用独立的Python源码文件，如：atr、dv2、kst、rsi、tsi、zlind等。这些独立的金融指标源码文件，是自定义Indicator的最佳模板。

图13-3至图13-7是Indicator指标类中常用指标的模块库架构图。