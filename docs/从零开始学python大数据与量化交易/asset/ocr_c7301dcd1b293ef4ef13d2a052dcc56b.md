# 从零开始学 Python 大数据与量化交易

```
In [1]: stocks = get_index_stocks('000300.XSHG')
... stocks
Out[1]: ['000001.XSHG', '000002.XSHG', '000008.XSHG', '000060.XSHG', '000053.XSHG', '000052.XSHG', '000100.XSHG', '000167.XSHG', '000166.XSHG', '000333.XSHG', '000338.XSHG', '000402.XSHG', '000413.XSHG', '000415.XSHG', '000423.XSHG', '000425.XSHG', '000503.XSHG', '000538.XSHG', '000610.XSHG', ...]
```

图 11.12 显示沪深 300 指数的所有股票代码

# 11.7 获取行业成分股代码的 get_industry_stocks() 函数

get_industry_stocks ()函数可以获取在给定日期一个行业的所有股票代码，其语法格式如下：

```
get_industry_stocks(industry_code, date=None)
```

参数 date 和返回值，都与 get_index_stocks ()函数相同，这里不再多说。
参数 industry_code 是指行业代码。行业代码也有很多，常用行业代码及意义如下。

+   A01：农业。
A02：林业。
A03：畜牧业。
A04：渔业。
A05：农、林、牧、渔服务业。
B06：煤炭开采和洗选业。
B07：石油和天然气开采业。
B08：黑色金属矿采选业。
B09：有色金属矿采选业。
C13：农副食品加工业。
C14：食品制造业。