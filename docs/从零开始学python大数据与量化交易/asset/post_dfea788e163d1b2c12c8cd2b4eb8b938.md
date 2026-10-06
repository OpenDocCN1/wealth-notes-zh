
## 第 16 章 Python 量化交易策略的实战案例

```
def trade(context):
    if len(g.stocks) > 0:
        code = g.code
        # 如持仓股票不是最低市净率的股票，则卖出
        for stock in context.portfolio.positions.keys():
            if stock != code:
                order_target(stock,0)
        # 持仓该股票
        if len(context.portfolio.positions) > 0:
            return
        else:
            order_value(code, context.portfolio.cash)
```

### 16.7.4 药药股轮动量化交易策略的回测

在回测之前，设置开始时间为 2016 年 4 月 8 日，结束时间为 2018 年 6 月 1 日。设置账户交易资金为 10 万元，交易频率为每天，如图 16.16 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_369_0.png)

图 16.16 设置回测时间和频率

在设置好各项参数后，单击“编译运行”按钮，编译运行医药股轮动量化交易策略。运行完毕后，看到策略收益和基准收益对比图表，如图 16.17 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_369_1.png)

图 16.17 策略收益和基准收益对比图表

从这里可以看到，基准收益为 13.81%，而策略收益为 17.23%，策略年度收益为 8.49%。这表明医药股轮动量化交易策略是一个不错的策略。

## 16.8 中市值股票量化交易策略实战案例

中市值股票量化交易策略，即筛选出市值介于 300 亿～600 亿元的股票，选取其中市值最小的 3 只股票，然后在每天开盘时买入，持有 5 个交易日，然后调仓。

### 16.8.1 编写初始化函数

双击“实战案例”文件夹，然后单击“新建策略”按钮，在弹出的下拉菜单中选择“股票策略”命令，新建一个“股票策略”，然后双击策略名称，重新命名为“中市值股票量化交易策略”。

删除文本框中所有代码，重新编写代码。首先导入函数，然后编写初始化函数，具体代码如下：

```
# 导入函数库
import jqdata
## 初始化函数，设定要操作成功的股票、基准等
def initialize(context):
    # 设定沪深300作为基准
    set_benchmark('000300.XSHG')
    # True 为开启动态复权模式，使用真实价格交易
    set_option('use_real_price', True)
    # 设定成交量比例
    set_option('order_volume_ratio', 1)
    # 股票类交易手续费是：买入时佣金万分之三，卖出时佣金万分之三加千分之一印花税，每笔交易佣金最低扣 5 块钱
    set_order_cost(OrderCost(open_tax=0, close_tax=0.001,\
                            open_commission=0.0003, close_commission=0.0003,\
                            close_today_commission=0, min_commission=5), type='stock')
    # 持仓数量
    g.stocknum = 3
    # 交易日计时器
    g.days = 0
    # 调仓频率
    g.refresh_rate = 5
    # 运行函数
    run_daily(trade, 'every_bar')
```

### 16.8.2 编写选股函数

选股函数，即筛选出市值介于 20 亿～30 亿元的股票，选取其中市值最小的 3 只股票，具体代码如下：

```
def check_stocks(context):
    # 设定查询条件
    q = query(
        valuation.code,
        valuation.market_cap
    ).filter(
        valuation.market_cap.between(300,600)
    ).order_by(
        valuation.market_cap.asc()
    )
    # 选出低市值的股票，构成buylist
    df = get_fundamentals(q)
    buylist = list(df['code'])
    # 过滤停牌股票
    buylist = filter_paused_stock(buylist)
    return buylist[:g.stocknum]
```

### 16.8.3 编写过滤停牌股票函数

过滤停牌股票函数，具体代码如下：

```
def filter_paused_stock(stock_list):
    current_data = get_current_data()
    return [stock for stock in stock_list if not current_data[stock].paused]
```

### 16.8.4 编写交易函数

交易函数，即每天开盘时买入，持有 5 个交易日，然后调仓，具体代码如下：

```
def trade(context):
    if g.days%g.refresh_rate == 0:
        ## 获取持仓列表
        sell_list = list(context.portfolio.positions.keys())
        # 如果有持仓，则卖出
        if len(sell_list) > 0 :
            for stock in sell_list:
                order_target_value(stock, 0)
        ## 分配资金
        if len(context.portfolio.positions) < g.stocknum :
            Num = g.stocknum - len(context.portfolio.positions)
            Cash = context.portfolio.cash/Num
        else:
            Cash = 0
        ## 选股
        stock_list = check_stocks(context)
```

```
## 买入股票
for stock in stock_list:
    if len(context.portfolio.positions.keys()) < g.stocknum:
        order_value(stock, Cash)
# 天计数加一
g.days = 1
else:
    g.days += 1
```

从零开始学 Python 大数据与量化交易

### 16.8.5 中市值股票量化交易策略的回测

在回测之前，设置开始时间为 2016 年 6 月 1 日，结束时间为 2018 年 6 月 1 日。设置账户交易资金为 10 万元，交易频率为每天，如图 16.18 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_372_0.png)

图 16.18 设置回测时间和频率

在设置好各项参数后，单击 “编译运行” 按钮，就可以编译运行中市值股票量化交易策略。运行完毕后，看到策略收益和基准收益对比图表，如图 16.19 所示。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_372_1.png)

图 16.19 策略收益和基准收益对比图表

从这里可以看到，基准收益为 18.96%，而策略收益为-32.17%，策略年度收益为-17.94%。这表明中市值股票量化交易策略不是一个好的策略。

## 16.9 低估价值股量化交易策略实战案例

低估价值股，在这里要满足 3 个条件，分别是市净率小于 2、负债比例高于市场平均值、企业的流动资产至少是流动负债的 1.2 倍。

另外，本策略还实现一年几次调仓，即 1 月、4 月、7 月、10 月。

### 16.9.1 编写初始化函数

双击“实战案例”文件夹，然后单击“新建策略”按钮，在弹出的下拉菜单中选择“股票策略”命令，新建一个“股票策略”，然后双击策略名称，重新命名为“低估价值股量化交易策略”。

删除文本框中所有代码，重新编写代码。首先导入函数，然后编写初始化函数，具体代码如下：

```
# 导入函数库
import jqdata
## 初始化函数，设定要操作的股票、基准等
def initialize(context):
    # 设定指数
    g.stockindex = '000300.XSHG'
    # 设定沪深 300 作为基准
    set_benchmark('000300.XSHG')
    # True 为开启动态复权模式，使用真实价格交易
    set_option('use_real_price', True)
    # 设定成交量比例
    set_option('order_volume_ratio', 1)
    # 股票类交易手续费是：买入时佣金万分之三，卖出时佣金万分之三加千分之一印花税，每笔交易佣金最低扣 5 块钱
    set_order_cost(OrderCost(open_tax=0, close_tax=0.001, \
                            open_commission=0.0003, close_commission=0.0003,\
                            close_today_commission=0, min_commission=5), type='stock')
    # 最大持仓数量
    g.stocknum = 10
    ## 自动设定调仓月份（如需使用自动，注销下段）
    f = 4  # 调仓频率，即每 3 个月调一次仓
    log.info(range(1,13,12/f))
    g.Transfer_date = range(1,13,12/f)
    # 按月调用程序
    run_monthly(trade, monthday=20, time='open')
```

### 16.9.2 编写选股函数

选股函数，即筛选出市净率小于 2、负债比例高于市场平均值、企业的流动资产至少是流动负债的 1.2 倍的股票，具体代码如下：

```
def check_stocks(context):
    # 获取沪深成分股
    security = get_index_stocks(g.stockindex)
    Stocks = get_fundamentals(query(
        valuation.code,
        valuation.pb_ratio,
        balance.total_assets,
        balance.total_liability,
        balance.total_current_assets,
        balance.total_current_liability
    ).filter(
        valuation.code.in_(security),
        valuation.pb_ratio < 2, # 市净率低于 2
        balance.total_current_assets/balance.total_current_liability > 1.2 # 流动资产至少是流动负债的 1.2 倍
    ))
    # 计算股票的负债比例
    Stocks['Debt_Asset'] = Stocks['total_liability']/Stocks['total_assets']
    # 获取负债比率的市场均值
    me = Stocks['Debt_Asset'].median()
    # 获取满足上述条件的股票列表
    Codes = Stocks[Stocks['Debt_Asset'] > me].code
    return list(Codes)
```

### 16.9.3 编写交易函数

交易函数，即根据月份判断是否卖出股票，如果当前月是 1 月、4 月、7 月或 10 月，则卖出手中的股票，然后分配资金再买入市净率小于 2、负债比例高于市场平均值、企业的流动资产至少是流动负债 1.2 倍的股票。需要注意，持有股票最多 10 只，具体代码如下：

```
def trade(context):
    # 获取当前月份
    months = context.current_dt.month
    # 如果当前月为交易月
    if months in g.Transfer_date:
        ## 获得 Buylist
        Buylist = check_stocks(context)
        ## 卖出
        if len(context.portfolio.positions) > 0:
            for stock in context.portfolio.positions.keys():
                if stock not in Buylist:
                    order_target(stock, 0)
        ## 分配资金
        if len(context.portfolio.positions) < g.stocknum:
```

```
Num = g.stocknum  -len(context.portfolio.positions)
Cash = context.portfolio.cash/Num
else:
    Cash = 0
## 买入
if len(Buylist) > 0:
    for stock in Buylist:
        if stock not in context.portfolio.positions.keys():
            order_value(stock,Cash)
else:
    return
```

### 16.9.4 低估价值股量化交易策略的回测

在回测之前，设置开始时间为 2016 年 6 月 1 日，结束时间为 2018 年 6 月 1 日。设置账户交易资金为 10 万元，交易频率为每天，如图 16.20 所示。

图 16.20 设置回测时间和频率

在设置好各项参数后，单击 “编译运行” 按钮，编译运行低估价值股量化交易策略。运行完毕后，看到策略收益和基准收益对比图表，如图 16.21 所示。

图 16.21 策略收益和基准收益对比图表

可以看到，基准收益为 18.96%，而策略收益为 39.77%，策略年度收益为 14.28%。这表明低估价值股量化交易策略是一个不错的策略。

![](img/261a0e647c121c3aa8af17bcacd0d7ae_376_0.png)

从零开始学 Python 大数据与量化交易

## 学习心得

## 理财投资名言

> 第一是不要害怕冒险，第二是冒险时不要押上全部家当。
—— 乔治·索罗斯

> 风险来自你不知道自己正在做什么！
—— 沃伦·巴菲特

> 投资不仅仅是一种行为，更是一种带有哲学意味的东西。
—— 约翰·坎贝尔

> 不进行研究的投资，就像打扑克从不看牌一样，必然失败！
—— 彼得·林奇

> 始终遵守你自己的投资计划和规划，这将加强良好的自我控制。
—— 伯妮斯·科恩

一本让投资者快速提高证券投资实战能力的秘籍。

100多个实战案例全面解析量化交易的热点难点问题。

全面解决证券分析难题，创造证券投资获利奇迹！

![](img/261a0e647c121c3aa8af17bcacd0d7ae_377_0.png)

![](img/261a0e647c121c3aa8af17bcacd0d7ae_377_1.png)
