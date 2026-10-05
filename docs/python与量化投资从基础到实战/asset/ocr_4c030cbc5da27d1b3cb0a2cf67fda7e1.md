```
'''
if isinstance(data, list):
    data = np.array(data)
if isinstance(data, pd.Series):
    data = data.values

def get_mdd(values): # values为np.array 的净值曲线，初始资金为 1
    dd = [values[i:].min() / values[i] - 1 for i in range(len(values))]
    return abs(min(dd))

if not isinstance(data, pd.DataFrame):
    return get_mdd(data)
else:
    return data.apply(get_mdd)
```

定义计算策略评价的各项指标的函数如下：

```
def cal_indicators(df_daily_return):
    '''
    功能：给定 daily return，计算各组合的评价指标，包括：年化收益率、年化标准差、夏普值、最大回撤
    输入：
    df_daily_return pd.DataFrame, index 为升序排列的日期, columns 为各组合名称, value 为 daily_return
    '''
    df_cum_value = (df_daily_return + 1).cumprod()
    res = pd.DataFrame(index=['年化收益率','年化标准差','夏普值','最大回撤'], columns=df_daily_return.columns, data=0.0)
    res.loc['年化收益率'] = (df_daily_return.mean() * 250).apply(lambda x: '%.2f%%' % (x*100))
    res.loc['年化标准差'] = (df_daily_return.std() * np.sqrt(250)).apply(lambda x: '%.2f%%' % (x*100))
    res.loc['夏普值'] = (df_daily_return.mean() / df_daily_return.std() * np.sqrt(250)).apply(lambda x: np.round(x, 2))
    res.loc['最大回撤'] = cal_maxdrawdown(df_cum_value).apply(lambda x: '%.2f%%' % (x*100))
    return res
```

# 2）历史回测

回测区间为从 2007 年 1 月 1 日至今，选取上证 50 成分股为标的，调仓频率为每季度末换仓，用过去 500 个交易日的数据估计协方差矩阵。