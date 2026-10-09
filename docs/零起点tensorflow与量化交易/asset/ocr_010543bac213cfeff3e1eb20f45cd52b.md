```
```python
df_train, df_test, x_train, y_test = zdat.df_xed_xtyp2x(df_train, df_test, '3', k0=99.5, k9=100.5)
y_train, y_test = pd.get_dummies(df_train['y']).values, pd.get_dummies(df_test['y']).values
#
typ_lst = y_train[0]
num_in, num_out = len(xlst), len(typ_lst)
print('\nnum_in,num_out:', num_in, num_out)
#
print('\ndf_test.tail()')
print(df_test.tail())
print('\nx_train.shape,', x_train.shape)
print('\ntype(x_train),', type(x_train))
```
```

读取神经网络模型训练数据和测试数据，并调用 ztools_data 模块中的 **df_xed_xtyp2** 函数，把数据分为上涨、下跌、平稳 3 种类型。

第 2.1 组代码对应的输出信息如下：

```
```
# 2.1，读取数据
xlst
['open', 'high', 'low', 'close', 'volume', 'avg', 'ma_2', 'ma_3', 'ma_5',
'ma_10', 'ma_15', 'ma_20', 'ma_25', 'ma_30', 'ma_50', 'ma_100', 'xyear',
'xmonth', 'xday', 'xday_week', 'xday_year', 'xweek_year']

num_in, num_out: 22 3

df_test.tail()
       open  high  low close volume  avg ma_2 ma_3 ma_5 ma_10 ... xmonth xday xday_week xday_year xweek_year  y price
3.310 ...  1   1    3    1   0  3.000 3.280
3.310   100.915     3
3.066 3.310 3.310 3.270 3.290 1654384.00 3.290 3.290 3.280 3.280
3.310 ...  1   1    3    1   0  3.000 3.290
3.310   100.608     3
3.067 3.320 3.320 3.300 3.310 1210668.00 3.310 3.300 3.290 3.290
3.300 ...  1   1    3    1   0  2.000 3.310
```
```