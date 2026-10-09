```
ax2.bar(np.array(Date)[np.array(seriesData.Close>=seriesData.Open)]
        ,height=seriesData.iloc[:,4][np.array(seriesData.Close>=seriesData.Open)]
        ,color='r',align='center')
ax2.bar(np.array(Date)[np.array(seriesData.Close<seriesData.Open)]
        ,height=seriesData.iloc[:,4][np.array(seriesData.Close<seriesData.Open)]
        ,color='g',align='center')
ax2.set_title(barTitle)
return plt.show()
```

将上述 candleVolume() 函数的定义代码放入之前定义的 candle 模块内，通过导入 candle 模块来调用此函数。下面绘制长江证券 2015 年 4 月份的 K 线与成交量图。

```
#从candle 模块中导入candleVolume()函数
In [3]: from candle import candleVolume
In [4]: CJSecurities1=CJSecurities['2015-04-01':'2015-04-30']
In [5]: candleVolume(CJSecurities1,candleTitle='长江证券2015年4月份K线与成交量图',\\n 
                   barTitle='长江证券2015年4月份涨跌幅一览')
...:
```

# ### 33.2.2 价涨量平

“价涨量平”表明当价格在持续上涨时，股票的成交量却不再上涨而是保持持平状态。通常成交量的变化会带动价格的变化，因此当成交量变化不大时，市场中价格上涨的动能有可能会不再增加。价涨量平体现出一种量价背离关系，量价的背离释放出市场可能反转的信号。上涨行情中价格持续上涨，成交量保持不变，可能预示着价格即将到达顶部。如果成交量继续持平或者减少，价格可能会从顶部反转下跌走势。此外，价涨量平的量价背离不一定都是反转情形，也有可能是市场处于调整期。市场经过一番洗盘之后，有可能会出现价涨量平的情形。还有一种情形是，当价格上涨过猛（比如价格连续多天涨停），投资者对市场一致看好，买盘很多，但卖盘很少，成交量呈现比较温和的状态，此时成交量会失去反映市场动能的意义。价涨量平也体现出市场处于指标修复期。

```
#获取长江证券股票2015年1月15日到2015年2月15日的日度交易数据；
In [6]: CJSecurities2=CJSecurities['2015-01-15':'2015-02-15']
#画K线图
In [7]: candleVolume(CJSecurities2,candleTitle='长江证券2015年1月15日到2月15日K线图',\
                   barTitle='长江证券2015年1月和2月涨跌幅一览')
...:
```

如图 33.2 所示。观察长江证券 2015 年 1 月 20 日附近的蜡烛图和成交量，股价在上涨，而成交量在持平，1 月 21 日成交量下跌，股价继续上涨，最终股价在 1 月 23 日出现了见顶形态，趋势由上升行情转成下跌行情。

# ### 33.2.3 价涨量缩

“价涨量缩”说明价格上涨成交量却在缩小，“价涨量缩”也是一种量价背离关系。市场横盘调整完有可能会出现价涨量缩的情形，经过一番调整，一批投资者被洗盘出局，在