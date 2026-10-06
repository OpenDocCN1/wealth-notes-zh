
# 从零开始学 Python 大数据与量化交易

get_trades()

该函数获取当天的所有成交记录。需要注意的是，一个订单可能分多次成交。获取成交信息函数的实例代码如下：

```
#自定义每个交易日结束运行函数
def after_trading_end(context):
    #得到当天所有成交记录
    trades = get_trades()
    for _trade in trades.values():
        log.info(_trade.trade_id)
```

## 10.5 日志 log

日志可以用来记录应用程序的状态、错误和信息消息，也经常作为调试程序的工具。下面讲解如何设定 log 级别及最常用的 log.info。

### 10.5.1 设定 log 的级别

设定 log 级别语法格式如下：

```
log.set_level(name, level)
```

设置不同种类的 log 的级别，低于这个级别的 log 不会输出。所有 log 的默认级别是 debug。

各项参数意义如下。

name: 字符串，但 log 种类只有 3 种，分别是 order、history、strategy。order 表示调用 order 系列 API 产生的 log；history 表示调用 history 系列 API 产生的 log；strategy 表示程序员自己在策略代码中输入的 log。

level: 字符串，必须是 debug、info、warning、error 中的一个。级别顺序是 debug < info < warning < error。各级别的语法格式如下：

```
log.error(content)
log.warn(content)
log.info(content)
log.debug(content)
```

设定 log 级别的实例代码如下：

```
# 过滤掉 order 系列 API 产生的比 error 级别低的 log
log.set_level('order', 'error')
```

### 10.5.2  log.info

日志 log 最常用的方法是 info，该方法的语法格式如下：

```
log.info(content)
```

log.info 等同于 print 输出的结果。

参数 content，可以是字符串、对象等。

log.info 的实例代码如下：

```
log.info(history(10)) # 打印出 history(10)返回的结果
log.info("Selling %s, amount=%s", security, amount) # 打印出一个格式化后的字符串
```

## 10.6  常用对象

在股票量化交易策略中还会用到一些常用的对象，如 Order 对象、全局对象 g、Trade 对象等。下面详细讲解常用对象。

### 10.6.1  Order 对象

Order 对象的常用属性如下。

amount: 下单数量，不管是买还是卖，都是正数。
filled: 已经成交的股票数量，正数。
security: 股票代码。
order_id: 订单 ID。
price: 平均成交价格，已经成交的股票平均价格（一个订单可能分多次成交）。
avg_cost: 卖出时表示下卖单前的此股票的持仓成本，用来计算此次卖出的收益。买入时表示此次买入的均价（等同于 price）。
side: 用来指定开多单还是空单。如果其值是 long，代表开多单；如果其值是 short，代表开空单。
action: 用来指定是开仓还是平仓。如果其值是 open，代表开仓；如果其值是 close，代表平仓。
add_time: 订单添加时间。

### 10.6.2  全局对象 g

全局对象 g 用来存储用户的各类可被 pickle.dumps 函数序列化的全局数据。

在模拟盘中，如果中途进程中断，会使用 pickle.dumps 序列化所有的 g 下面的变量内容，保存到磁盘中，再启动时模拟盘就不会有任何数据影响。如果没有用 g 声明，会出现模拟盘重启后，变量数据丢失的问题。

如果不想 g 中的某个变量被序列化，可以让变量以 “__” 开头，这样这个变量在序列化时就会被忽略。

全局对象 g 的示例代码如下：

```
def initialize(context):
    g.security = "000001.XSHE"
    g.count = 1
    g.flag = 0
def process_initialize(context):
    # 保存不能被序列化的对象，进程每次重启都初始化
    g.__q = query(valuation)
def handle_data(context, data):
    log.info(g.security)
    log.info(g.count)
    log.info(g.flag)
```

### 10.6.3 Trade 对象

Trade 对象用于记录订单的一次交易。但需要注意的是，一个订单可能分多次交易。Trade 对象的常用属性如下。

- time: 交易时间。
- amount: 交易数量。
- price: 交易价格。
- trade_id: 交易记录 id。
- order_id: 对应的订单 id。

### 10.6.4 tick 对象

tick 中的信息是在 tick 事件发生时盘面的一个快照。tick 对象的常用属性如下。

- code: 标的的代码。
- datetime: tick 发生的时间。
- current: 最新价。
- high: 最高价。
- low: 最低价。
- volume: 截至当前时刻的成交量。

### 10.6.5 Context 对象

amount: 截至当前时刻的成交额。
position: 截至当前时刻的持仓量，只适用于期货 tick 对象。
a1_v ~ a5_v: 卖一量到卖五量，对于期货，只有卖一量。
a1_p ~ a5_p: 卖一价到卖五价，对于期货，只有卖一量。
b1_v ~ b5_v: 买一量到买五量，对于期货，只有买一量。
b1_p ~ b5_p: 买一价到买五价，对于期货，只有买一价。

Context 对象，即量化交易策略回测对象，其常用属性如下。

subportfolios: 当前单个操作仓位的资金和标的信息。

portfolio: 账户信息，即 subportfolios 的汇总信息。

current_dt: 当前单位时间的开始时间。如果是按天回测，那么开始时间是 hour = 9，minute = 30，second = microsecond = 0；如果是按分钟回测，那么开始时间是 second = microsecond = 0。

previous_date: 前一个交易日。

universe: 查询 set_universe()设定的股票池，如["000001.XSHE","600000.XSHG"]。

run_params: 表示此次运行的参数，有以下属性。

start_date: 回测/模拟开始日期。

end_date: 回测/模拟结束日期。

type: 运行方式。如果其值是 simple_backtest，表示回测是通过单击“编译运行”按钮运行；如果其值是 full_backtest，表示回测是通过单击“运行回测”按钮运行；如果其值是 sim_trade，表示模拟交易。

frequency: 运行频率，只能是 day、minute 或 tick。

为了让从其他平台迁移过来的程序员更顺手地使用系统，对此对象也做了和 g 一样的处理。

- (1) 可以添加自己的变量，每次进程关闭时持久保存，进程重启时恢复。
- (2) 以“__”开头的变量不会被持久保存。
- (3) 如果添加的变量与系统冲突，将覆盖系统变量，如果想恢复系统变量，就要删除自己的变量，示例代码如下：

```
def handle_data(context, data):
    # 执行下面的语句之后，context.portfolio 的整数 1 将被覆盖并持久保存
    context.portfolio = 1
    log.info(context.portfolio)
    # 要恢复系统的变量，只需要使用下面的语句即可
```

```
del context.portfolio
# 此时，context.portfolio 将变成账户信息。
log.info(context.portfolio.portfolio_value)
```

(4) 以后可能会往 context 添加新的变量来支持更多功能，为了减少不必要的误解，还是建议大家使用 g。

Context 对象的示例代码如下：

```
def handle_data(context, data):
    #获得当前回测相关时间
    year = context.current_dt.year
    month = context.current_dt.month
    day = context.current_dt.day
    hour = context.current_dt.hour
    minute = context.current_dt.minute
    second = context.current_dt.second
    #得到"年-月-日"格式
    date = context.current_dt.strftime("%Y-%m-%d")
    #得到周几
    weekday = context.current_dt.isoweekday()
    # 获取账户的持仓价值
    positions_value = context.portfolio.positions_value
    # 获取仓位 subportfolios[0]的可用资金
    available_cash = context.subportfolios[0].available_cash
    # 获取 subportfolios[0]中多头仓位的 security 的持仓成本
    hold_cost = context.subportfolios[0].long_positions[security].hold_cost
```

### 10.6.6 Position 对象

Position 对象是持有的某个标的的信息，其常用属性如下。

- security: 标的代码。
price: 最新行情价格。
avg_cost: 开仓均价，买入标的的加权平均价。
hold_cost: 持仓成本，针对期货有效。
init_time: 建仓时间，格式为 datetime.datetime。
transact_time: 最后交易时间，格式为 datetime.datetime。
total_amount: 总仓位，但不包括挂单冻结仓位。
closeable_amount: 可卖出的仓位。
today_amount: 今天开的仓位。
locked_amount: 挂单冻结仓位。

value: 标的价值，计算方法是：price * total_amount * multiplier，其中股票、基金的 multiplier 为 1，期货为相应的合约乘数。

side: 用来指定开多单还是空单。如果其值是 long，代表开多单；如果其值是 short，代表开空单。

pindex: 仓位索引。

### 10.6.7 SubPortfolio 对象

SubPortfolio 对象是某个仓位的资金和标的信息，其常用属性如下。
inout_cash: 累计出入金。例如，初始资金 1000 元，后来转移出去 100 元，则这个值是 1000 − 100=900 元。
available_cash: 可用资金，可用来购买证券的资金。
transferable_cash: 可取资金，即可以提现的资金，不包括今日卖出证券所得资金。
locked_cash: 挂单锁住资金。
type: 账户所属类型。
long_positions: 多单的仓位。
short_positions: 空单的仓位。
positions_value: 持仓价值，只有股票基金才有持仓价值，期货为 0。
total_value: 总资产，包括现金、保证金、仓位的总价值，可用来计算收益。
total_liability: 总负债，等于融资负债、融券负债、利息总负债的总和。
net_value: 净资产，等于总资产减去总负债。
cash_liability: 融资负债。
sec_liability: 融券负债。
interest: 利息总负债。
maintenance_margin_rate: 维持担保比例。
available_margin: 融资融券可用保证金。
margin: 保证金，股票、基金保证金都为 100%；融资融券保证金为 0；期货保证金会实时更新，总是等于当前期货价值×保证金比例，当保证金不足时，强制平仓。平仓原则是：亏损多的（相对于开仓均价）先平仓。

### 10.6.8 Portfolio 对象

Portfolio 对象是所有标的操作仓位的信息汇总，其常用属性如下。

inout_cash: 累计出入金。例如，初始资金 1000 元，后来转移出去 100 元，则这个值是 1000-100=900 元。

available_cash: 可用资金，可用来购买证券的资金。

transferable_cash: 可取资金，即可以提现的资金，不包括今日卖出证券所得资金。

locked_cash: 挂单锁住资金。

margin: 保证金，股票、基金保证金都为 100%。

positions: 等同于 long_positions。

long_positions: 多单的仓位。

short_positions: 空单的仓位。

total_value: 总的权益，包括现金、保证金、仓位的总价值，可用来计算收益。

returns: 总权益的累计收益。

starting_cash: 初始资金。

positions_value: 持仓价值，只有股票基金才有持仓价值，期货为 0。

locked_cash_by_purchase: 基金申购未完成所冻结的金额。

locked_cash_by_redeem: 基金赎回未到账的金额。

locked_amount_by_redeem: 基金赎回时冻结的份额。

### 10.6.9 SecurityUnitData 对象

SecurityUnitData 对象是一个单位时间内的股票数据，其常用属性如下。

security: 股票代码。如 000001.XSHE。

returns: 股票在这个单位时间的相对收益比例。

open: 时间段开始时价格。

close: 时间段结束时价格。

low: 最低价。

high: 最高价。

volume: 成交的股票数量。

money: 成交的金额。

factor: 前复权因子。利用前复权因子可以计算出原始价格，方法是价格除以 factor，即原始价格=close÷factor。

high_limit: 涨停价。

low_limit: 跌停价。

avg: 这段时间的平均价。

## 第 11 章

# Python 量化交易策略的 获取数据函数

要编写股票量化交易策略，就必须获取股票的数据（如收盘价、5 日均价、上一时间的点价格），还要选出操作的股票，这些都需要用到获取数据函数。本章共讲解 12 个获取数据函数，分别是 history()函数、attribute_history ()函数、get_fundamentals ()函数、get_fundamentals_continuously ()函数、get_current_data ()函数、get_index_stocks ()函数、get_industry_stocks() 函数、get_concept_stocks () 函数、get_all_securities() 函数、get_security_info () 函数、get_billboard_list ()函数和 get_locked_shares ()函数。

## 11.1 获取股票数据的 history()函数

history()函数可以查看一只股票或多只股票历史行情数据，其语法格式如下：

```
history(count, unit='1d', field='avg', security_list=None, df=True, skip_paused=False, fq='pre')
```

需要注意的是，在获取天数据时，不包括当天的 数据，即使是在收盘后。

关于停牌：因为获取了多只股票的数据，可能有的股票停牌有的没有，为了保持时间轴的一致性，默认没有跳过停牌的日期，停牌时使用停牌前的数据填充。

### 11.1.1 各项参数的意义

history()函数共有 7 项，各项参数意义如下。

- 1. count
参数 count 表示数量，即返回结果集的行数。

- 2. unit
参数 unit 表示单位时间长度，几天或者几分钟，即只支持 Xd 和 Xm，其中 X 是一个正整数。
“Xd” 表示具体几天，如 10d，表示单位时间长度为 10 天。
“Xm” 表示具体几分钟，如 10m，表示单位时间长度为 10 分钟。
需要注意的是，当 X > 1 时，fields 只支持["open"、"close"、"high"、"low"、"volume"、"money"]这几个标准字段。
其中： open 表示时间段股票的开始时价格。 close 表示时间段股票的结束时价格。 high 表示时间段股票的最高价格。 low 表示时间段股票的最低价格。 volume 表示时间段股票的成交量。 money 表示时间段股票的成交金额。

- 3. field
参数 field，表示获取的数据类型，支持 SecurityUnitData 里面的所有基本属性，包括 ["open"、"close", "low", "high", "volume", "money", "factor", "high_limit", "low_limit", "avg", "pre_close", "paused"]。
前面 6 个字段的意义同参数 unit，这里不再多说。下面看一下其他字段的意义。 factor: 前复权因子。 high_limit: 涨停价。
