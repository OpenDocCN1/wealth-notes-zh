```
... 

In [35]: r1, r2 = my_arithmetic(7, 5)

In [36]: r1, r2
Out[36]: (12, 2)
```

# 7.2 函数的参数

在调用函数时，传入的实参一般会按照位置顺序与形参绑定。这种方式被称为传递 “位置参数（Positional Argument）”。例如，我们写 my_subtraction(7, 2) 时，Python 会将 7 赋值给 minuend（minuend = 7），将 2 赋值给 subtrahend（subtrahend = 2）。

```
In [29]: my_subtraction(7, 2)
Out[29]: 5
```

如果写成 my_subtraction(2, 7)，计算结果则完全不同：

```
In [33]: my_subtraction(2, 7)
Out[33]: -5
```

+   - 我们也可以在调用时写明实参与形参的绑定关系，如 my_subtraction(minuend = 2, subtrahend = 1) 或 my_subtraction(subtrahend = 1, minuend = 2)；这种方式则被称为传递 “关键字参数（Keyword Argument）”。

```
In [30]: my_subtraction(minuend = 7, subtrahend = 2)
Out[30]: 5
```

关键字参数传递的顺序可以是任意的：

```
In [31]: my_subtraction(subtrahend = 2, minuend = 7)
Out[31]: 5
```

如果需要同时传递位置参数与关键字参数，则位置参数的顺序应先于关键字参数，否则 Python 会报错。

```
In [32]: my_subtraction(subtrahend = 2, 7)
File "<python-input-32-dd2e54ea2a99>"，line 1
my_subtraction(subtrahend = 2, 7)
^
SyntaxError: positional argument follows keyword argument
```

定义函数时，可以为参数指定默认值。例如：

```
In [7]: def my_print(arg1, arg2 = 'World!'):
...: print(arg1, arg2)
...

In [8]: my_print('Hi')
Hi World!
```