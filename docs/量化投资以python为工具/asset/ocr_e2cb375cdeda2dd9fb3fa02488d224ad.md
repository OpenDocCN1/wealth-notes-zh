# ### 5.1.6 成员运算符

成员运算表示某一对象是否属于一个序列的元素之一，成员运算符有 in 和 not in 两种。给定变量 a 和 b:

+   - 若 a 是序列 b 中的元素之一，则表达式“a in b”的返回值为 True，表达式“a not in b”的返回值为 False；

+   - 若 a 不是序列 b 中的元素之一，则表达式“a in b”的返回值为 False，表达式“a not in b”的返回值为 True。

```
```
In [1]: a=2
In [2]: b={3,4,2}
In [3]: c={2,3,4}
In [4]: d={2,4,5}
In [5]: a in b
Out[5]: True
In [6]: a in c
Out[6]: True
In [7]: a in d
Out[7]: True
In [8]: '2' in b
Out[8]: False
In [9]: '2' in c
Out[9]: False
In [10]: '2' in d
Out[10]: False
```
```

对于字符串序列来说，成员运算符的左侧的操作数必须为字符串类型，否则会报错。

```
```
In [11]: e='abc2'
In [12]: a not in e
Traceback (most recent call last):

 File "ipython-input-27-818824315870", line 1, in <module>
 a not in e

TypeError: 'in <string>' requires string as left operand, not int

In [13]: '2' not in e
Out[13]: False
```
```