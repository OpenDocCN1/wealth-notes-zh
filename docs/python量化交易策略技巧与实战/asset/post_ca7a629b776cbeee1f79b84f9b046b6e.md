
# 图 13.22 基本面因子的 “换手分析”

## 第14章

## Python 量化交易策略实战案例

Python 量化交易策略实战案例，可以提高投资者对量化交易策略的综合认识，有利于投资者真正掌握量化交易技巧，从而学以致用。

本章主要内容包括:

- MA 均线量化交易策略实战案例
- 多均线量化交易策略实战案例
- MACD 指标量化交易策略实战案例
- KD 指标量化交易策略实战案例
- BOLL 指标量化交易策略实战案例
- 多股票持仓量化交易策略实战案例
- 医药股轮动量化交易策略实战案例
- 小市值股票量化交易策略实战案例
- 机器算法多因子量化交易策略实战案例

### 14.1 MA 均线量化交易策略实战案例

MA 指标是反映价格运行趋势的重要指标，其运行趋势一旦形成，将在一段时间内继续保持。趋势运行所形成的高点或低点又分别具有阻挡或支撑作用，因此均线指标所在的点位往往是十分重要的支撑或阻力位，这就提供了买进或卖出的有利时机，均线系统的价值也正在于此。下面利用 MA 均线指标来编写一个量化交易策略，实现当价格高于 5 日均线平均价格 10% 时买入，当价格低于 5 日平均价格 90% 时卖出。

#### 14.1.1 编写初始化函数

成功登录聚宽 JoinQuant 量化交易平台后，单击导航栏中的 “策略研究” → “策略列表” 命令，打开 “策略列表” 页面，如图 14.1 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_341_0.png)

图 14.1 “策略列表” 页面

单击 “新建文件夹” 按钮，即可新建一个文件夹，将其命名为 “量化交易策略实战案例”，如图 14.2 所示。

单击 “新建策略” → “股票策略” 命令，即可新建一个股票策略，然后双击策略名称，将其重命名为 “MA 均线量化交易策略实战案例”，如图 14.3 所示。

图 14.2 新建文件夹并命名

图 14.3 新建股票策略并命名为 “MA 均线量化交易策略”

为了更好地利用 Python 编辑量化交易策略，删除文本框中的所有代码，重新编写代码。首先，导入函数包，即可利用前面讲解的各类函数和 MA 均线指标，具体代码如下：

```
import jqdata
from jqllib.technical_analysis import *
```

接下来，编写初始化函数。为了便于学习，这里的初始化函数相对简单，具体代码如下：

```
# 初始化函数，设定要操作的股票、基准等等
def initialize(context):
    # 定义一个全局变量，保存要操作的股票
    # 000002 (股票：万科A)
    g.security = '000002.XSHE'
    # 设定沪深300作为基准
    set_benchmark('000300.XSHG')
```

```
# 开启动态复权模式(真实价格)
set_option('use_real_price', True)
```

#### 14.1.2 编写单位时间调用的函数

单位时间调用的函数，即每个单位时间调用一次的函数（如果按天回测，则每天调用一次，如果按分钟，则每分钟调用一次），具体代码如下：

```
def handle_data(context, data):
    security = g.security
    # 获取股票的收盘价
    close_data = attribute_history(security, 5, '1d', ['close'])
    # 取得过去5天的平均价格
    MA5 = close_data['close'].mean()
    # 取得上一时间点价格
    current_price = close_data['close'][-1]
    # 取得当前的现金
    cash = context.portfolio.cash
    # 如果当前有余额，并且上一时间点价格高出5天平均价10%
    if current_price > 1.1*MA5:
        # 用所有 cash 买入股票
        order_value(security, cash)
        # 记录这次买入
        log.info("Buying %s" % (security))
    # 如果上一时间点价格低于5天平均价，并且前有头寸
    elif current_price < 0.9*MA5 and context.portfolio.positions[security].closeable_amount > 0:
        # 卖出所有股票,使这只股票的最终持有量为0
        order_target(security, 0)
        # 记录这次卖出
        log.info("Selling %s" % (security))
```

这时调用 attribute_history()函数获取最近 5 天的股票收盘价，利用 mean()函数获取最近 5 天的平均价，再获取上一时间点价格。

在此，买入股票的条件是当前有余额，并且上一时间点价格高出 5 天平均价 10%，具体代码如下：

```
if current_price > 1.1*MA5
```

卖出股票的条件是上一时间点价格低出 5 天平均价 90%，并且目前有头寸，具体代码如下：

```
elif current_price < 0.9*MA5 and context.portfolio.positions[security].closeable_amount > 0:
```

#### 14.1.3 MA 均线量化交易策略的回测

在回测之前，先设置开始时间为 “2016-06-01”，结束时间为 “2018-06-01”，账户交易资金为 “100 000” 元，交易频率为 “每天”，如图 14.4 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_344_0.png)

图 14.4 设置回测时间和频率

设置好各项参数后，单击 “编译运行” 按钮，即可编译运行 MA 均线量化交易策略，运行完毕后，即可看到策略收益和基准收益对比图表，如图 14.5 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_344_1.png)

图 14.5 策略收益和基准收益对比图表

从图 14.5 可知，基准收益为 18.96%，而策略收益为 7.49%，策略年化收益为 3.77%。这表明 MA 均线量化交易策略对基准，即沪深 300 来说，是一个不错的策略。但对万科 A（000002）来说，策略收益不高，策略年化收益也不高，不是一个好策略。

### 14.2 多均线量化交易策略实战案例

在炒股实战中，往往多条均线一起使用，如 5 日、10 日和 30 日均线。下面利用多均线来编写一个量化交易策略，实现功能如下：

当 5 日均线大于 10 日均线、10 日均线大于 30 日均线且收盘价格高于 5 日均线价格的 1.01 倍时，买入股票；当 5 日均线小于 10 日均线、10 日均线小于 30 日均线且收盘价格低于 5 日均线价格的 0.99 倍时，卖出股票。

#### 14.2.1 编写初始化函数

双击“量化交易策略实战案例”文件夹，然后单击“新建策略”→“股票策略”命令，即可新建一个股票策略，然后双击策略名称，将其重命名为“多均线量化交易策略实战案例”。

删除文本框中所有代码，重新编写代码。首先导入函数，然后编写初始化函数，具体代码如下：

```
# 导入函数库
import jqdata
from jqllib.technical_analysis import *
def initialize(context):
    # 定义一个全局变量，保存要操作的股票
    # 000538(股票:云南白药)
    g.security = '000538.XSHE'
    # 设定沪深 300 作为基准
    set_benchmark('000300.XSHG')
    # True 为开启动态复权模式，使用其实价格交易
    set_option('use_real_price', True)
    # 设定成交量比例
    set_option('order_volume_ratio', 1)
    # 股票类交易手续费是：买入时佣金的万分之三，卖出时佣金万分之三加千分之一印花税
    # 每笔交易佣金最低扣 5 元
    set_order_cost(OrderCost(open_tax=0, close_tax=0.001, \
                            open_commission=0.0003, close_commission=0.0003,\
                            close_today_commission=0, min_commission=5), type='stock')
    # 运行函数
    run_daily(trade, 'every_bar')
```

注意在初始化函数中，加入了股票每笔交易时的手续费，即买入时佣金的万分之三，卖出时佣金的万分之三加千分之一印花税，每笔交易佣金最低扣 5 元。

#### 14.2.2 编写交易程序函数

在定时函数 run_daily 中，调用了交易程序函数 trade，该函数的具体代码如下：

```
def trade(context):
    security = g.security
```

```
# 设定均线
n1 = 5
n2 = 10
n3 = 30
# 获取股票的收盘价
close_data = attribute_history(security, n3+2, '1d', ['close'],df=False)
# 取得过去 ma_n1 天的平均价格
ma_n1 = close_data['close'][-n1:].mean()
# 取得过去 ma_n2 天的平均价格
ma_n2 = close_data['close'][-n2:].mean()
# 取得过去 ma_n3 天的平均价格
ma_n3 = close_data['close'][-n3:].mean()
# 取得上一时间点价格
current_price = close_data['close'][-1]
# 取得当前的现金
cash = context.portfolio.cash
# 如果当前有余额，并且 n1 日均线大于 n2 日均线，n2 日均线大于 n3 日均线，上一时间点价格
# 高出五天平均价 1%
if ma_n1 > ma_n2 and ma_n2 > ma_n3 and current_price> 1.01*ma_n1:
    # 用所有 cash 买入股票
    order_value(security, cash)
    # 记录这次买入
    log.info("Buying %s" % (security))
# 如果 n1 日均线小于 n2 日均线，n2 日均线小于 n3 日均线，上一时间点价格低于 5 天
# 平均价 1%，并且目前有头寸
elif ma_n1 < ma_n2 and ma_n2 < ma_n3 and current_price< 0.99*ma_n1 and context.portfolio.positions[security].closeable_amount > 0:
    # 全部卖出
    order_target(security, 0)
    # 记录这次卖出
    log.info("Selling %s" % (security))
```

利用 attribute_history()函数获取股票数据，然后获取 5 日、10 日、30 日均线的值及上一时间点价格。

在此，买入股票的条件是如果当前有余额，并且 n1 日均线大于 n2 日均线，n2 日均线大于 n3 日均线，上一时间点价格高出五天平均价 1%，代码如下：

```
if ma_n1 > ma_n2 and ma_n2 > ma_n3 and current_price> 1.01*ma_n1:
```

卖出股票的条件是如果 n1 日均线小于 n2 日均线，n2 日均线小于 n3 日均线，上一时间点价格低于 5 天平均价 1%，并且目前有头寸，代码如下：

```
elif ma_n1 < ma_n2 and ma_n2 < ma_n3 and current_price< 0.99*ma_n1 and context.portfolio.positions[security].closeable_amount > 0:
```

#### 14.2.3 多均线量化交易策略的回测

在回测之前，先设置开始时间为 2016 年 6 月 1 日，结束时间为 2018 年 6 月 1 日，账户交易资金为 100 000 元，交易频率为每天，如图 14.6 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_347_0.png)

设置好各项参数后，单击“编译运行”按钮，即可编译运行多均线量化交易策略，运行完毕后，就可以看到策略收益和基准收益对比图表，如图 14.7 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_347_1.png)

从图 14.7 可知，基准收益为 18.96%，而策略收益为 31.85%，策略年化收益为 15.22%。这表明多均线量化交易策略对基准，即沪深 300 来说，是一个不错的策略；对云南白药（000538）来说，策略收益较高，策略年化收益也较高，也是一个不错的策略。

### 14.3 MACD 指标量化交易策略实战案例

MACD 指标是股市中最经典、最常用的技术指标。下面利用 MACD 指标来编写一个简单的量化交易策略，实现 DIFF、DEA 均为正，DIFF 向上突破 DEA 时，买入股票；DIFF、DEA 均为负，DIFF 向下跌破 DEA 时，卖出股票。

#### 14.3.1 编写初始化函数

双击“量化交易策略实战案例”文件夹，然后单击“新建策略”→“股票策略”命令，即可新建一个股票策略，然后双击策略名称，将其重命名为“MACD 指标量化交易策略实战案例”。

删除文本框中所有代码，重新编写代码。首先导入函数，然后编写初始化函数，具体代码如下：

```
import jqdata
from jqlib.technical_analysis import *
def initialize(context):
    # 定义一个全局变量，保存要操作的股票
    # 000001(股票:平安银行)
    g.security = '000001.XSHE'
    # 设定沪深 300 作为基准
    set_benchmark('000300.XSHG')
    # 开启动态复权模式(真实价格)
    set_option('use_real_price', True)
```

#### 14.3.2 编写单位时间调用的函数

单位时间调用的函数，即每个单位时间调用一次的函数（如果按天回测，则每天调用一次，如果按分钟，则每分钟调用一次），具体代码如下：

```
def handle_data(context, data):
    #获取初始化中要操作的股票
    security = g.security
    #调用 MACD 函数，并获取股票的 MACD 指标的 DIF, DEA 和 MACD 的值
    macd_diff, macd_dea, macd_macd = MACD(security,check_date=context.current_dt, SHORT = 12, LONG = 26, MID = 9)
    # 取得当前的现金
    cash = context.portfolio.cash
    # 如果当前有余额，并且 DIFF、DEA 均为正，DIFF 向上突破 DEA
    if macd_diff > 0 and macd_dea >0 and macd_diff > macd_dea :
        # 用所有 cash 买入股票
        order_value(security, cash)
        # 记录这次买入
        log.info("买入股票 %s" % (security))
    # 如果 DIFF、DEA 均为负，DIFF 向下跌破 DEA，并且目前有头寸
    elif macd_diff < 0 and macd_dea < 0 and macd_diff < macd_dea and context.portfolio.positions[security].closeable_amount> 0:
        # 全部卖出
        order_target(security, 0)
```

```
# 记录这次卖出
log.info("卖出股票 %s" % (security))
```

需要注意该代码通过调用 MACD 函数，获得股票的 MACD 指标的 DIF、DEA 和 MACD 的值；还需要注意该代码中的参数 check_date=context.current_dt，表示获取当前回测日期。

在此，买入股票的条件是如果当前有余额，并且 DIFF、DEA 均为正，DIFF 向上突破 DEA，具体代码如下：

```
if macd_diff > 0 and macd_dea >0 and macd_diff > macd_dea :
```

卖出股票的条件是如果 DIFF、DEA 均为负，DIFF 向下跌破 DEA，并且目前有头寸，具体代码如下：

```
elif macd_diff < 0 and macd_dea < 0 and macd_diff < macd_dea and context.portfolio.positions[security].closeable_amount > 0:
```

#### 14.3.3 MACD 指标量化交易策略的回测

在回测之前，先设置开始时间为 2017 年 6 月 1 日，结束时间为 2018 年 5 月 4 日，账户交易资金为 100 000 元，交易频率为每天，如图 14.8 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_349_0.png)

图 14.8 设置回测时间和频率

设置好各项参数后，单击“编译运行”按钮，即可编译运行 MACD 指标量化交易策略，运行完毕后，即可看到策略收益和基准收益对比图表，如图 14.9 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_349_1.png)

图 14.9 策略收益和基准收益对比图表
