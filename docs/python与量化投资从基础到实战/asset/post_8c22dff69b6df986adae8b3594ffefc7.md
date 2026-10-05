
# Python 与量化投资：从基础到实战

```
Short Allowed: {}
Portfolio Return: {}
Portfolio Volatility: {}
Portfolio Weights: {}
""".format(risk_aversion, allow_short, r, v, "\n\t{}".format("\n\t".join("{}": {:.1%}".format(k, v) for k, v in weights_dict.items()))).strip()
```

```
return weights_dict
```

```
def get_maximum_sharpe_portfolio(return_table, riskfree_rate=0., allow_short=False, show_details=True):
    """
    计算最大效用组合，目标函数为：（期望年化收益率 − 无风险收益率）/ 期望年化方差

    Args:
        return_table (DataFrame)：收益率矩阵，列为资产，值为按日期升序排列的收益率
        riskfree_rate (float)：无风险收益率
        allow_short (bool)：是否允许卖空
        show_details (bool)：是否显示细节

    Returns:
        dict：最小方差组合的权重信息，键为资产名，值为权重
    """
```

```
import numpy as np
from cvxopt import matrix, solvers
```

```
assets = return_table.columns
n_asset = len(assets)
if n_asset < 2:
    output = describe(return_table, is_print=False)
    r = output['annualized_return'].iat[0, 0]
    v = output['annualized_volatility'].iat[0, 0]
    weights_dict = {assets[0]: 1.}
else:
    efs = get_efficient_frontier(return_table, allow_short=allow_short, n_samples=100)
    i_star = max(range(100), key=lambda x: (efs.at[x, "returns"] - riskfree_rate) / efs.at[x, "risks"])
    r = efs.at[i_star, "returns"]
    v = efs.at[i_star, "risks"]
```

```
weights_dict = efs.at[i_star, "weights"]

s = (r - riskfree_rate) / v

if show_details:
    print """
Maximum Sharpe Portfolio:
    Riskfree Rate: {}
    Short Allowed: {}
    Portfolio Return: {}
    Portfolio Volatility: {}
    Portfolio Sharpe: {}
    Portfolio Weights: {}
""".format(riskfree_rate, allow_short, r, v, s, "\n\t{}".format("\n\t".join("{}: {:.1%}".format(k, v) for k, v in weights_dict.items()))).strip()

return weights_dict
```

如下代码利用上述理论和函数构造了一个简单的均值方差模型示例：

```
start = '20130101'
end = '20160630'
indices = ['000300.ZICN', '000905.ZICN', '399006.ZICN', '000012.ZICN', '000013.ZICN']
df = DataAPI.MktIdxdGet(indexID=indices, beginDate=start, endDate=end, field="tradeDate,indexID,closeIndex")
df = df.pivot(index="tradeDate", columns="indexID", values="closeIndex")
for index in indices:
    df[index] = df[index] / df[index].shift() - 1.
return_table = df.dropna()

describe(return_table, is_print=True)
```

运行结果如下：

```
covariance_matrix:
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
indexID
000012.ZICN     0.000057      0.000009      0.000047      0.000089      0.000034
000013.ZICN     0.000009      0.000031      0.000108      0.000173      0.000179
000300.ZICN     0.000047      0.000108      0.080919      0.078297      0.071111
000905.ZICN     0.000089      0.000173      0.078297      0.106164      0.110480
399006.ZICN     0.000034      0.000179      0.071111      0.110480      0.148057
```

```
annualized_return:
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
0        0.045391      0.075276     0.068568     0.206761     0.408574

annualized_volatility:
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
0        0.007538      0.005588     0.284462     0.325829     0.384782

coefficient_matrix:
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
indexID
000012.ZICN  1.000000  0.222367  0.021862  0.036328  0.011754
000013.ZICN  0.222367  1.000000  0.067919  0.095261  0.083447
000300.ZICN  0.021862  0.067919  1.000000  0.844759  0.649678
000905.ZICN  0.036328  0.095261  0.844759  1.000000  0.881214
399006.ZICN  0.011754  0.083447  0.649678  0.881214  1.000000

{'annualized_return': indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
0        0.045391      0.075276     0.068568     0.206761     0.408574,
'annualized_volatility': indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
0        0.007538      0.005588     0.284462     0.325829     0.384782,
'coefficient_matrix':
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
indexID
000012.ZICN  1.000000  0.222367  0.021862  0.036328  0.011754
000013.ZICN  0.222367  1.000000  0.067919  0.095261  0.083447
000300.ZICN  0.021862  0.067919  1.000000  0.844759  0.649678
000905.ZICN  0.036328  0.095261  0.844759  1.000000  0.881214
399006.ZICN  0.011754  0.083447  0.649678  0.881214  1.000000,
'covariance_matrix':
indexID  000012.ZICN  000013.ZICN  000300.ZICN  000905.ZICN  399006.ZICN
indexID
000012.ZICN  0.000057  0.000009  0.000047  0.000089  0.000034
000013.ZICN  0.000009  0.000031  0.000108  0.000173  0.000179
000300.ZICN  0.000047  0.000108  0.080919  0.078297  0.071111
000905.ZICN  0.000089  0.000173  0.078297  0.106164  0.110480
399006.ZICN  0.000034  0.000179  0.071111  0.110480  0.148057}
```

对 efficient_frontier 的计算如下：

```
efficient_frontier = get_efficient_frontier(return_table, allow_short=False, n_samples=50)
efficient_frontier
```

计算结果如下：

|  | returns | risks | weights |
| 0 | 0.045391 | 0.007538 | {u'000300.ZICN': 1.15393430344e-08, u'000905.Z... | 
| 1 | 0.052803 | 0.006133 | {u'000300.ZICN': 0.000496525017029, u'000905.Z... | 
| 2 | 0.060215 | 0.005181 | {u'000300.ZICN': 0.000251697279178, u'000905.Z... | 
| 3 | 0.067627 | 0.004964 | {u'000300.ZICN': 0.000223375191279, u'000905.Z... | 
| 4 | 0.075038 | 0.005558 | {u'000300.ZICN': 0.000130923763251, u'000905.Z... | 
| 5 | 0.08245 | 0.010298 | {u'000300.ZICN': 8.53789819096e-07, u'000905.Z... | 
| 6 | 0.089862 | 0.018088 | {u'000300.ZICN': 5.32468255522e-06, u'000905.Z... | 
| 7 | 0.097274 | 0.02635 | {u'000300.ZICN': 9.02636326881e-07, u'000905.Z... | 
| 8 | 0.104686 | 0.034751 | {u'000300.ZICN': 2.02472361778e-07, u'000905.Z... | 
| 9 | 0.112098 | 0.043209 | {u'000300.ZICN': 9.28878058971e-08, u'000905.Z... | 
| 10 | 0.11951 | 0.051697 | {u'000300.ZICN': 4.57879968585e-08, u'000905.Z... | 
| 11 | 0.126922 | 0.060201 | {u'000300.ZICN': 2.40064060703e-08, u'000905.Z... | 
| 12 | 0.134334 | 0.068717 | {u'000300.ZICN': 1.37843225515e-06, u'000905.Z... | 
| 13 | 0.141746 | 0.077239 | {u'000300.ZICN': 8.4285683672e-07, u'000905.ZI... | 
| 14 | 0.149157 | 0.085766 | {u'000300.ZICN': 5.49491378923e-07, u'000905.Z... | 
| 15 | 0.156569 | 0.094297 | {u'000300.ZICN': 3.77948806475e-07, u'000905.Z... | 
| 16 | 0.163981 | 0.102831 | {u'000300.ZICN': 2.70306319752e-07, u'000905.Z... | 
| 17 | 0.171393 | 0.111366 | {u'000300.ZICN': 2.00553907455e-07, u'000905.Z... | 
| 18 | 0.178805 | 0.119904 | {u'000300.ZICN': 1.52644281534e-07, u'000905.Z... | 
| 19 | 0.186217 | 0.128443 | {u'000300.ZICN': 1.17142800495e-07, u'000905.Z... | 
| 20 | 0.193629 | 0.136982 | {u'000300.ZICN': 9.02783351161e-08, u'000905.Z... | 
| 21 | 0.201041 | 0.145523 | {u'000300.ZICN': 6.97438031359e-08, u'000905.Z... | 
| 22 | 0.208453 | 0.154064 | {u'000300.ZICN': 5.9059023565e-08, u'000905.ZI... | 
| 23 | 0.215865 | 0.162606 | {u'000300.ZICN': 5.19484511925e-08, u'000905.Z... | 
| 24 | 0.223276 | 0.171149 | {u'000300.ZICN': 4.55263038143e-08, u'000905.Z... | 

| 25 | 0.230688 | 0.179692 | {u'000300.ZICN': 3.98048975884e-08, u'000905.Z... |
| 26 | 0.2381 | 0.188235 | {u'000300.ZICN': 3.47759997238e-08, u'000905.Z... |
| 27 | 0.245512 | 0.196779 | {u'000300.ZICN': 3.04007403968e-08, u'000905.Z... |
| 28 | 0.252924 | 0.205323 | {u'000300.ZICN': 2.66383477155e-08, u'000905.Z... |
| 29 | 0.260336 | 0.213868 | {u'000300.ZICN': 2.34494796732e-08, u'000905.Z... |
| 30 | 0.267748 | 0.222412 | {u'000300.ZICN': 2.07935283905e-08, u'000905.Z... |
| 31 | 0.27516 | 0.230957 | {u'000300.ZICN': 1.86292415345e-08, u'000905.Z... |
| 32 | 0.282572 | 0.239502 | {u'000300.ZICN': 1.69144006915e-08, u'000905.Z... |
| 33 | 0.289984 | 0.248047 | {u'000300.ZICN': 1.56046914802e-08, u'000905.Z... |
| 34 | 0.297395 | 0.256592 | {u'000300.ZICN': 2.78591582895e-08, u'000905.Z... |
| 35 | 0.304807 | 0.265138 | {u'000300.ZICN': 2.40132032721e-08, u'000905.Z... |
| 36 | 0.312219 | 0.273683 | {u'000300.ZICN': 2.06266291619e-08, u'000905.Z... |
| 37 | 0.319631 | 0.282229 | {u'000300.ZICN': 1.77207719046e-08, u'000905.Z... |
| 38 | 0.327043 | 0.290774 | {u'000300.ZICN': 2.38027675854e-08, u'000905.Z... |
| 39 | 0.334455 | 0.29932 | {u'000300.ZICN': 3.21755527866e-08, u'000905.Z... |
| 40 | 0.341867 | 0.307866 | {u'000300.ZICN': 4.45262652532e-08, u'000905.Z... |
| 41 | 0.349279 | 0.316412 | {u'000300.ZICN': 6.45951915931e-08, u'000905.Z... |
| 42 | 0.356691 | 0.324958 | {u'000300.ZICN': 1.02657055122e-07, u'000905.Z... |
| 43 | 0.364103 | 0.333504 | {u'000300.ZICN': 1.87751964819e-09, u'000905.Z... |
| 44 | 0.371514 | 0.342051 | {u'000300.ZICN': 1.0304584484e-07, u'000905.ZI... |
| 45 | 0.378926 | 0.350597 | {u'000300.ZICN': 3.08767068474e-09, u'000905.Z... |
| 46 | 0.386338 | 0.359143 | {u'000300.ZICN': 1.40775462715e-08, u'000905.Z... |
| 47 | 0.39375 | 0.367689 | {u'000300.ZICN': 1.31902224778e-08, u'000905.Z... |
| 48 | 0.401162 | 0.376236 | {u'000300.ZICN': 7.95099027876e-09, u'000905.Z... |
| 49 | 0.408574 | 0.384782 | {u'000300.ZICN': -1.89291467521e-10, u'000905.... |

画出有效前沿图像：

```
draw_efficient_frontier(efficient_frontier)
```
