```
# ==============================
data = context.data
before_2_close = data['closePrice'][-2]
before_2_high = data['highPrice'][-2]
before_1_close = data['closePrice'][-1]
before_1_high = data['highPrice'][-1]
before_1_low = data['lowPrice'][-1]
before_1_open = data['openPrice'][-1]
# 观察卖出价
ssetup=context.high+0.35*(context.close-context.low)
# 观察买入价
bsetup=context.low-0.35*(context.high-context.close)
# 反转卖出价
senter=(1+0.07)/2*(context.high+context.low)-0.07*context.low
# 反转买入价
benter = (1+0.07)/2*(context.high+context.low)-0.07*context.high
# 突破买入价
bbreak = ssetup+0.25*(ssetup-bsetup)
# 突破卖出价
sbreak = bsetup-0.25*(ssetup-bsetup)

## 趋势
if before_2_close <= bbreak and before_1_close > bbreak:
    if long_position == 0:
        futures_account.order(symbol, amount, 'open')
    if short_position != 0:
        futures_account.order(symbol, short_position, 'close')
if before_2_close >= sbreak and before_1_close < sbreak:
    if short_position == 0:
        futures_account.order(symbol, -amount, 'open')
    if long_position != 0:
        futures_account.order(symbol, -long_position, 'close')
## 反转
### 多单反转
if before_1_high > ssetup and before_1_close > senter:
    context.count1 = 1
if context.count1 == 1 and before_1_close < senter:
    if long_position > 0:
        futures_account.order(symbol, ~long_position, 'close')
        futures_account.order(symbol, -amount, 'open')
### 空单反转
```