```
In [10]: b and a
Out[10]: 0

In [11]: b or 1
Out[11]: 1

In [12]: False and True
Out[12]: False
```

当然，我们还可以使用下面的形式：

```
In [13]: 0 and 33
Out[13]: 0
```

# ### 5.1.5 身份运算符

身份运算符有 is 和 is not 两种，其主要用于判断两个对象所对应的 id 是否相等。

+   - 如果两个变量 a 和 b 指向同一个存储对象，即变量 a 和 b 的 id 相等，则表达式“a is b”的返回值为 True，“a is not b”的返回值为 False；

+   - 如果两个变量 a 和 b 指向不同的存储对象，即变量 a 和 b 的 id 不相等，则表达式“a is b”的返回值为 False，“a is not b”的返回值为 True。

给变量 a 和 b 均赋值为 2，查看变量 a 和 b 的 id，并进行身份运算符操作。

```
In [1]: a=2

In [2]: b=2

In [3]: a==b
Out[3]: True

In [4]: id(a)
Out[4]: 1543565104

In [5]: id(b)
Out[5]: 1543565104

In [6]: a is b
Out[6]: True

In [7]: a is not b
Out[7]: False
```

先将变量 b 的值更改为 50，查看变量 a 和 b 的 id 变化情况：

```
In [8]: b=50

In [9]: id(b)
Out[9]: 1543566640  # 变量 b 的 id 发生变化

In [10]: id(a)
Out[10]: 1543565104  # 变量 a 的 id 不变
```