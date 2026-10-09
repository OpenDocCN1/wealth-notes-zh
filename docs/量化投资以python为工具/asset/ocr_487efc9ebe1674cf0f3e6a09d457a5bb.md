# 16.3.1 单因素方差分析

现在来考虑一个简单的问题，即行业对股票收益率的影响。直觉上，我们都认为行业对股票收益率有着重要的影响，不同行业的股票的平均收益率是不一样的。以美国为例，互联网等高科技行业的平均股票收益率就比其他行业要高。接下来我们用方差分析来验证一下这个根据经验形成的直觉。不过值得注意的是，方差分析的假设条件是各组别各反应变量之间是独立同分布的，这里只是为了说明Python实践方差分析的过程。

我们所用的数据为2014年的股票年收益率，如表 16.1 所示为该数据的前几项，

# 表 16.1 股票收益率表

<table> <tbody> <tr> <td>编码</td> <td>年份</td> <td>收益率</td> <td>行业</td> </tr> <tr> <td>000001</td> <td>2014</td> <td>0.57298</td> <td>货币金融服务</td> </tr> <tr> <td>000002</td> <td>2014</td> <td>0.827567</td> <td>房地产业</td> </tr> <tr> <td>000003</td> <td>2014</td> <td>0.336481</td> <td>医药制造业</td> </tr> <tr> <td>000004</td> <td>2014</td> <td>0.64</td> <td>房地产业</td> </tr> <tr> <td>000005</td> <td>2014</td> <td>0.477997</td> <td>房地产业</td> </tr> </tbody> </table>

接下来，用Python编写代码进行方差分析。

```
```python
# 获取数据
In [1]: import pandas as pd
    ...: import statsmodels.stats.anova as anova
    ...: from statsmodels.formula.api import ols
    ...:
In [2]: year_return=pd.read_csv('016\YRD_year.csv',\\
    ...:                        encoding='gbk')
    ...: year_return.head()
Out[2]:
   Code  Year    Return Industry
0     1  2014  0.572980 货币金融服务
1     2  2014  0.827567 房地产业
2     3  2014  0.336481 医药制造业
3     4  2014  0.640000  号纸行业
4     5  2014  0.477997 房地产业

# 按照进行方差分析:
In [3]: model=ols('Return~C(Industry)',\\
    ...:            data=year_return.dropna()).fit()
In [4]: table1 = anova.anova_lm(model)
In [5]: print(table1)
             df     sum_sq    mean_sq          F       PR(>F)
Industry   74   60.517228   0.817800   4.177614   4.382045e-28
Residual 2302  450.634318   0.195758       NaN        NaN
```
```

上述结果表明，$p=4.38e-028$，在0.05的显著性水平下，$p$值远远小于0.05，故我们应该拒绝原假设，认为不同行业股票2014年的年收益率是不一样的。因此，我们的直觉得到了验证，即行业是影响股票收益率的一个重要因素。