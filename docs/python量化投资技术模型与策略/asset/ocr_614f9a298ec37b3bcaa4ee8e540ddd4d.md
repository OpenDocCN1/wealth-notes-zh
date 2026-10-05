```
track[2]])
    factor.append([track[0],trading_day_low_pe_return-
    trading_day_high_
        pe_return])
    except KeyError:
        continue

hml = pd.DataFrame(factor,columns=['date','hml'])
```

至此，三因子模型需要的数据算是建立起来了，下面将所有的
数据整合到一起，代码如下：

```
factors = smb.merge(hml,on = ['date'],how = 'inner',copy =
False)
factors = factors.set_index(['date'])

#将市值因子整合到一起
factors['capm'] = sh
factors['const'] = 1

#对每一只股票进行回归，得出资产池内的单个资产的暴露情况
#假设所有的因子都是统计显著的
from sklearn.linear_model import LinearRegression

factor_loading = []
for stock_id in sz380return.columns:
    regression_data = factors.copy()
    regression_data[stock_id] = sz380return[stock_id]
    print(stock_id)
    model = LinearRegression(fit_intercept =
True).fit(regression_data[['capm',
    'smb','hml']],regression_data[stock_id])
    factor_loading.append([stock_id]+
[model.intercept_]+list(model.coef_))
#最后整理为DataFrame
factor_loading = pd.DataFrame(factor_loading,columns=
['stock_id','alpha','capm',
    'smb','hml'])
```