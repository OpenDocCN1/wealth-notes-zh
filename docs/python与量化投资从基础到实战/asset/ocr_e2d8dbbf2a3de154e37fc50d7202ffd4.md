致，在分钟频率上回测，但 context.now 包含小时、分钟等日内时间信息，context.current_date 不包含这些信息。

示例如下：

```
context.current_date
```

如下代码用于获取策略运行时的当前日期：

```
start = '2017-01-01'      # 在 2017 年 1 月 1 日开始回测
end = '2017-02-01'        # 在 2017 年 2 月 1 日结束回测
freq = 'd'
refresh_rate = 1

accounts = {
    'fantasy_account': AccountConfig(account_type='security',
capital_base=10000000)
}

def initialize(context):            # 初始化策略运行环境
    pass

def handle_data(context):           # 核心策略逻辑
    account = context.get_account('fantasy_account')

    print context.current_date
```

如下展示了部分调仓日期：

```
2017-01-03 00:00:00
2017-01-04 00:00:00
2017-01-05 00:00:00
...
2017-01-25 00:00:00
2017-01-26 00:00:00
```

# 3. previous_date（回测日期的前一交易日）

用于获取当前回测日期的前一交易日，返回 datetime，只能在 handle_data 方法中使用，并且不允许修改。用法同 current_date 一致，示例如下：

```
context.previous_date
```