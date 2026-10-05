```
raise ValueError('direction should be back/forward!!!')

def cal_maxdrawdown(data):
    """
    功能: 给定净值数据 (list, np.array, pd.Series, pd.DataFrame), 返回最大回撤
    输入:
        data, list/np.array/pd.Series/pd.DataFrame, 净值曲线, 初始金为 1
    输出:
        list/np.array/pd.Series 返回 float
        pd.DataFrame 返回 pd.DataFrame, index 为 DataFrame.columns
    """

    if isinstance(data, list):
        data = np.array(data)
    if isinstance(data, pd.Series):
        data = data.values

    def get_mdd(values): # values 为 np.array 的净值曲线, 初始资金为 1
        dd = [values[i:].min() / values[i] - 1 for i in range(len(values))]
        return abs(min(dd))

    if not isinstance(data, pd.DataFrame):
        return get_mdd(data)
    else:
        return data.apply(get_mdd)

def cal_indicators(df_daily_return):
    """
    功能: 给定 daily return, 计算各组合的评价指标, 包括: 年化收益率、年化标准差、夏普值、最大回撤
    输入:
        df_daily_return pd.DataFrame, index 为升序排列的日期, columns 为各组合名称, value 为 daily_return
    """

    df_cum_value = (df_daily_return + 1).cumprod()
    res = pd.DataFrame(index=['年化收益率','年化标准差','夏普值','最大回撤'],
columns=df_daily_return.columns, data=0.0)
    res.loc['年化收益率'] = (df_daily_return.mean() * 250).apply(lambda x:
```