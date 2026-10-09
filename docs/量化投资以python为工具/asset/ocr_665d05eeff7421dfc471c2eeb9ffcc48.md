```
DataFrame.fillna(value=None, method=None, axis=None, inplace=False, limit=None)
```

函数中参数 value 是在缺失值处填充的值，可以是数值数字，也可以是字符串；method 是填充的方式，默认为 None，也可以取值为 ffill、pad、bfill 或 backfill，其中 ffill/pad 是用行或列方向上的上一个观测值来填充缺失值，bfill/backfill 是用行或列方向上的下一个观测值来填充；axis 与 method 配合使用，指定行（axis=1）或列（axis=0）的方向；limit=None 时，会填充连续的缺失值，如果指定数值的话，比如 limit=2，只会依次填充连续 NaN 值的指定数字个数（比如 2 个）；若 inplace=False 则不会变更原 DataFrame，若 inplace=True，则会改变原 DataFrame。

```
# 先构造含有 NaN 值的 DataFrame df4
In [1]: df4=pd.DataFrame(np.random.rand(5,4),
                        index=list('abcde'),
                        columns=list('ABCD'))
```

```
In [2]: df4.ix[':', 'A']=np.nan
...: df4.ix['b': 'd', 'C']=np.nan
```

```
In [3]: df4
Out[3]:
      A          B          C          D
a 0.620082 0.393647 0.875339 0.157148
b 0.138891 0.770243 NaN 0.490713
c NaN 0.250412 NaN 0.056830
d 0.895170 0.506699 NaN 0.575992
e 0.950164 0.374731 0.142002 0.570523
```

```
# 指定数值填充
In [4]: df4.fillna(0)
Out[4]:
      A          B          C          D
a 0.620082 0.393647 0.875339 0.157148
b 0.138891 0.770243 0.000000 0.490713
c 0.000000 0.250412 0.000000 0.056830
d 0.895170 0.506699 0.000000 0.575992
e 0.950164 0.374731 0.142002 0.570523
```

```
# 前一个观测值填充，列方向上的
In [5]: df4.fillna(method='ffill')
Out[5]:
      A          B          C          D
a 0.620082 0.393647 0.875339 0.157148
b 0.138891 0.770243 0.875339 0.490713
c 0.138891 0.250412 0.875339 0.056830
d 0.895170 0.506699 0.875339 0.575992
e 0.950164 0.374731 0.142002 0.570523
```

```
# 后一个观测值填充，列方向上
In [6]: df4.fillna(method='bfill')
Out[6]:
      A          B          C          D
a 0.620082 0.393647 0.875339 0.157148
b 0.138891 0.770243 0.142002 0.490713
```