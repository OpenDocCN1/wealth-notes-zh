```
```
In [2]: a=list()
In [3]: for i in 'python' :
...:        a.append(i+'python')
...:        print(a)
...:
['p']
['ppython', 'pypython']
['ppython', 'pypython', 'tpython']
['ppython', 'pypython', 'tpython', 'hpython']
['ppython', 'pypython', 'tpython', 'hpython', 'opython']
['ppython', 'pypython', 'tpython', 'hpython', 'opython', 'npython']
```
```

如果语句“print(a)”没有缩进，则会出现不同的 print 结果：

```
```
In [4]: a=list()
In [5]: for i in 'python':
...:        a.append(i+'python')
...: print(a)
...:
['ppython', 'pypython', 'tpython', 'hpython', 'opython', 'npython']
```
```

上述语句还可以写出较为简单的形式：

```
```
In [6]: a=[i+'python' for i in 'python']
In [7]: a
Out[7]: ['ppython', 'pypython', 'tpython', 'hpython', 'opython', 'npython']
```
```

再举一例，如果要判断一个列表中 10 的索引值，可以使用如下代码：

```
```
In [8]: list1=[2,10,34,3,10,20,10]
In [9]: [i for i in range(len(list1)) if list1[i]==10]
Out[9]: [1, 4, 6]
```
```

# 6.3.2 while 循环

在 Python 中，while 循环语句的形式为：

```
```
while expression:
  statements
```
```

当 while 循环的 expression 为 True 时，执行 statements 代表的语句或者语句块内容；当 expression 为 False 时，跳出循环，直接执行 statements 下面一行的代码。

```
```
In [1]: a=0
In [2]: while a<4:
...:        a=a+1
...:        print(a+26)
...: print(a)
```
```