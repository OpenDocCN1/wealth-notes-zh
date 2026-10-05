```
unit='perValue'), slippage = Slippage(value=0.0, unit='perValue'),

    'futures_account': AccountConfig(account_type='futures',
capital_base=10000000, commission = Commission(buycost=0.001, sellcost=0.002,
unit='perValue'), slippage = Slippage(value=0.0, unit='perValue'), margin_rate
= 0.1)
}

def initialize(context):        # 初始化策略运行环境
    pass

def handle_data(context):        # 核心策略逻辑
    account1 = context.get_account('security_account')
    account2 = context.get_account('futures_account')
```

# 1. AccountConfig（账户配置）

用于配置单个交易账户，在策略初始化时会根据账户的配置创建对应的交易账户。其参数如下。

+   ○ 必选参数: account_type capital_base。
○ 可选参数: position_base cost_base commission slippage。
○ 期货专用参数: margin_rate。
○ 场外基金专用参数: dividend_method。

# 2. account_type（账户类型设置）

用于设置交易账户类型，为 str 类型，支持 security 股票和场内基金、futures 期货、otc_fund 场外基金（不含货币基金）、index 指数。

```
account_type = 'security'
```

# 3. capital_base（初始资金设置）

用于设置交易账户初始资金，为 float 或 int 类型。示例如下：

```
capital_base = 100000
```