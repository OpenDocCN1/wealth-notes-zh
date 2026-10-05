```
df['Adj Close']=df['close']
df.columns=['Date','Open','High','Low','Close','Volume','Adj Close']
df.to_csv(code+'.csv',index=False)
```

这里我们用一个最简单的例子来说明如何使用PyAlgoTrade编写一个回测程序。策略的逻辑是计算N天的移动平均线，如果收盘价上穿移动平均线，就开多仓，如果收盘价下穿移动平均线就平仓。

首先，导入需要使用的模块，这些模块在后续都会使用到，代码如下：

```
import tushare as ts
from pyalgotrade import strategy
from pyalgotrade.barfeed import yahoofeed
from pyalgotrade.technical import ma
from pyalgotrade.technical import cross
from pyalgotrade import plotter
from pyalgotrade.tools import yahoofinance
from pyalgotrade.stratanalyzer import sharpe
```

一个回测策略就是一个类。以下是回测类的代码：

```
class MACross(strategy.BacktestingStrategy):
    def __init__(self, feed, instrument, length):
        super(MACross, self).__init__(feed)
        self.__instrument = instrument
        self.__position = None
        # 使用复权后的数据作为价格输入
        self.setUseAdjustedValues(True)
        self.__prices = feed[instrument].getPriceDataSeries()
        self.__sma = ma.SMA(self.__prices, length)
    def getSMA(self):
        return self.__sma
    def onEnterOk(self, position):
```