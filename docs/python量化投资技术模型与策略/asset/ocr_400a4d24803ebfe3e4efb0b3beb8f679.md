式中：

d₁ = [ln(S₀ / K) + (r + σ² / 2) T] / (σ √T)  (9-12)

d₂ = [ln(S₀ / K) + (r - σ² / 2) T] / (σ √T) = d₁ - σ √T  (9-13)

函数 N(x) 为标准正态分布的累积概率分布函数。c 与 p 分别为欧式看涨期权与看跌期权的价格，S₀ 为股票在 0 时刻的价格，K 为执行价格，r 为连续复利的无风险利率，σ 为股票价格的波动率，T 为期权的期限。

下面我们用 Python 来实现期权定价的函数。option_pricer 函数共有 6 个参数，每个参数在注释里面都有说明。此外，这里用到了 scipy.stats 的 norm 模块，程序代码具体如下：

```
def option_pricer(call_or_put, spot, strike, maturity, r, vol):
    """
    参数:
        call_or_put: 1代表计算看涨期权价格，0代表计算看跌期权价格
        spot: 标的资产价格
        strike: 执行价格
        maturity: 到期时间（年化）
        r: 无风险利率
        vol: 波动率
    返回:
    相应期权的价格
    """
        d1 = (log(spot/strike) + (r +
    0.5*vol*vol)*maturity)/vol/sqrt(maturity)
```