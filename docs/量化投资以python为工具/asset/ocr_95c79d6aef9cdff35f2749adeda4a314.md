```
      price times amount )
stock value equals toprice times amount
```

```
In [4]: 36\\
      ....: +65
Out[4]: 101
```

对于字符串来说，可以用单引号（’）、双引号（“ ”）以及三引号（‘‘‘ ”’”）来表示。在三引号中，一个逻辑行可以跨行编写占据多个物理行。举例如下：

```
In [5]: 'stock value equals to
    File "<python-input-10-efbe70f9455e>", line 1
     'stock value equals to
          _
SyntaxError: EOL while scanning string literal
In [6]: "stock value equals to\\
      ....: price times amount"
Out[6]: 'stock value equals toprice times amount'
```

```
In [7]: '''stock value equals to
      ....: price times amount'''
Out[7]: 'stock value equals to\nprice times amount'
```

# ### 2.3.2 Python 缩进

在 Python 中不是用花括号 {} 来表示语句块（Block），而是通过缩进来区分。缩进的空格数没有严格的定义，但是在一个语句块中所有语句必须使用相同空格数的缩进表示一个逻辑行序列。前面创建的test.py 中若代码没有缩进，执行此文件时会出现错误 IndentationError。

```
[Anaconda3] C:\\Users\PCPC>cd d:\\python quant\\part1

[Anaconda3] C:\\Users\PCPC>cd:

[Anaconda3] d:\\python quant\\part1>test.py
  File "d:\\python quant\\part1\\test.py", line 5
  if i%2==0:
  ^
IndentationError: expected an indented block

[Anaconda3] d:\\python quant\\part1>
```

不同的代码块或者缩进等级（Indentation Level）使用不同的空格数来区分。如果缩排使用不当，即使不会出现错误，也会出现不同的执行结果。分别创建 test1.py和 test2.py，两个文件中的代码分别为：

```
#test1.py文件中的代码
# --- coding: UTF-8 ---
stockPrice1=76
stockPrice2=80
```