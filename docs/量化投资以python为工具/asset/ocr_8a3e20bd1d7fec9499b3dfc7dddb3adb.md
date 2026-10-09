```
In [22]: plt.subplot(2,2,1)
 ... : plt.scatter(HXThrFac.HXRet,HXThrFac.RiskPremium2)
 ... : plt.subplot(2,2,2)
 ... : plt.scatter(HXThrFac.HXRet,HXThrFac.SMB2)
 ... : plt.subplot(2,2,3)
 ... : plt.scatter(HXThrFac.HXRet,HXThrFac.HML2)
Out[22]: <matplotlib.collections.PathCollection at 0x7f6378770978>
```

![](bbox=[0.1, 0.24, 0.93, 0.52])

接下来，我们将华夏银行的股票收益率与三个因子变量建立多元回归模型。从回归结果来看，华夏银行 2014 年 1 月至 2015 年 4 月股票的收益率对三因子模型中的市场投资组合风险溢酬因子、账面市值比因子是敏感的，也就是说这两个因子可以部分解释华夏银行的收益率变动，而市值因子的系数不显著地异于 0，解释能力不够强。

```
In [23]: import statsmodels.api as sm
In [24]: regThrFac=sm.OLS(HXThrFac.HXRet,sm.add_constant(HXThrFac.iloc[:,1:4]))
In [25]: result=regThrFac.fit()
In [26]: result.summary()
Out[26]:
<class 'statsmodels.iolib.summary.Summary'>
"""
           OLS Regression Results
==============================================================================
Dep. Variable:                 HXRet   R-squared:                       0.605
Model:                         OLS   Adj. R-squared:                    0.592
Method:                 Least Squares   F-statistic:                    232.5
Date:                Mon, 21 Mar 2016   Prob (F-statistic):          1.43e-73
Time:                        19:29:29   Log-Likelihood:                929.94
No. Observations:                 310          AIC:                      -1852.
"""
```