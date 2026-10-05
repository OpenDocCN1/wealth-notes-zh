```
In [4]: data = IndexData['closeIndex'] # 上证指数
    m = 10 # 我们检验 10 个自相关系数
    acf,q,p = sm.tsa.acf(data,nlags=m,qstat=True)  ## 计算自相关系数及 p-value
    out = np.c_[range(1,11), acf[1:], q, p]
    output=pd.DataFrame(out, columns=['lag', "AC", "Q", "P-value"])
    output = output.set_index('lag')
    output
```

Out[4]:

|  | AC | Q | P-value |
|---|---|---|---|
| lag |  |  |  |
| 1.0 | 0.977190 | 366.688991 | 9.842648e-82 |
| 2.0 | 0.951513 | 715.277906 | 4.779432e-156 |
| 3.0 | 0.927073 | 1047.065270 | 1.109192e-226 |
| 4.0 | 0.902993 | 1362.675600 | 8.565497e-294 |
| 5.0 | 0.878258 | 1662.026278 | 0.000000e+00 |
| 6.0 | 0.857131 | 1947.908436 | 0.000000e+00 |
| 7.0 | 0.836825 | 2221.134260 | 0.000000e+00 |
| 8.0 | 0.812991 | 2479.709003 | 0.000000e+00 |
| 9.0 | 0.789723 | 2724.350491 | 0.000000e+00 |
| 10.0 | 0.766245 | 2955.283132 | 0.000000e+00 |

我们取显著性水平为 0.05，可以看出，所有 p-value 都小于 0.05，所以我们拒绝原假设 H0。因此，我们认为该序列（即上证指数序列）是序列相关的。

再来看看同期上证指数的日收益率序列：

```
In [5]: data2 = IndexData['CHGPct'] # 上证指数日涨跌
    m = 10 # 我们检验 10 个自相关系数
    acf,q,p = sm.tsa.acf(data2,nlags=m,qstat=True)  ## 计算自相关系数及 p-value
    out = np.c_[range(1,11), acf[1:], q, p]
    output=pd.DataFrame(out, columns=['lag', "AC", "Q", "P-value"])
    output = output.set_index('lag')
```