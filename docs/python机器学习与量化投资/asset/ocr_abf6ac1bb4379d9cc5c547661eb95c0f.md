# Python 机器学习与二化投资

```
xlist, ysgn = ['x1','x2','x3','x4'], 'xid'
x, y = df[xlist], df[ysgn]
print('\n3\n xlist', xlist)
print('ysgn', ysgn)
print('x')
print(x.tail())
print('y')
print(y.tail())
```

对应的输出信息是：

```
3# xlist, ['x1', 'x2', 'x3', 'x4']
ysgn, xid
x
    x1  x2  x3  x4
245 6.7 3.0 5.2 2.3
246 6.3 2.5 5.0 1.9
247 6.5 3.0 5.2 2.0
248 6.2 3.4 5.4 2.3
249 5.9 3.0 5.1 1.8
y
245 1
246 1
247 1
248 1
249 1
Name: xid, dtype: int64
```

第4组代码，生成 x_train、x_test、y_train、y_test 数据，下面相关数据的格式信息：

```
4
x_train, x_test, y_train, y_test = train_test_split(x, y, random_state=1)
x_test.index.name, y_test.index.name = 'xid', 'xid'
print('\n4') type •)
```