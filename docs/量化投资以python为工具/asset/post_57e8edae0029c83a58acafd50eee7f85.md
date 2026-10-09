
内置函数属于 dir(__builtins__) 返回结果中的部分元素，dir(__builtins__) 返回值还包括一些内置错误类型、属性等其他内容。Python 内置函数有很多，本小节主要讲述与运算相关的内置函数。

类比于运算符 Python 中的一些内置函数也可以实现某些运算符能实现的运算功能，比如求和、求幂、整除、取模等运算。

```python
In [1]: sum([6,2,0]) # 求序列元素之和
Out[1]: 8.0

In [2]: pow(6,2) # 幂运算
Out[2]: 36

In [3]: divmod(6,2) # 整除与取模
Out[3]: (3, 0)

In [4]: (6//2, 6%2)
Out[4]: (3, 0)
```

如表 5.6 所列展示了 Python 数据运算相关的内置函数，调用函数时，需要传入函数要操作对象的取值才能进行相关数据操作。

### 表 5.6 运算功能相关的内置函数

| 函数名称 | 功能 | 使用举例 |
| :--- | :--- | :--- |
| sum(iterable, start=0,/) | 一个可迭代对象的所有元素相加 | sum([6,2,0]) |
| pow(x, y, z=None,/) | 若有三个参数 x、y 和 z，运算为 x**y%z；若只有两个参数 x 和 y，运算为 x**y | pow(6,2) |
| divmod(x, y,/) | 返回值为元组，运算相当于(x//y, x%y) | divmod(6,2) |
| abs(x,/) | 返回参数 x 的绝对值 | abs(-6) |
| all(iterable,/) | 可迭代对象所有元素的布尔值都为 True 时，返回 True；若可迭代对象为空时，也返回 True | all([3>2, 6<9]) |
| any(iterable,/) | 可迭代对象任一元素的布尔值为 True 时，返回 True；若可迭代对象为空时，返回 False | any([3>2, 6<9]) |
| max(...) | 求最大值 | max(6,9) |
| min(...) | 求最小值 | min(6,9) |
| round(x,n) | 求与参数 x 最接近的数，预设返回整数，n 有取值时代表返回数的小数点位数 | round(3.84, 1) |

举例如下：

```python
In [5]: abs(-6) # 求 -6 的绝对值
Out[5]: 6

# 列表对象 [3>2, 6<9] 有两个元素，分别是表达式 3>2 的返回值和
# 表达式 6<9 的返回值
In [6]: all([3>2, 6<9])
Out[6]: True

#列表对象[2,3,5,6,7]中元素的布尔值均为True
In [7]: all([2,3,5,6,7])
Out[7]: True

In [8]: any([2,3,5,6,7])
Out[8]: True

In [9]: any([])  #[]为空列表对象
Out[9]: False

In [10]: max(6,9)  #6和9均为函数的参数
Out[10]: 9

In [11]: max([2,3,5,6,7]) #列表[2,3,5,6,7]为可迭代对象
Out[11]: 7

In [12]: min(6,9)
Out[12]: 6

In [13]: min([2,3,5,6,7])
Out[13]: 2

In [14]: round(3.84,1)
Out[14]: 3.8
```

## 习题

- 1. 判断下列运算的结果：
  - (a) `3+4`, `3.4*4`
  - (b) `3//4`, `id(['a',3,True,'ccd'][1])==id(3)`
  - (c) `3==4 in [1,'345',3+4j,4 in [1,2,3]]`, `{3==4*4.5*2 is 0} in [3,4,'Tom','c' in 'comic']`
- 2. 比较 is 和 == 的异同。
- 3. 怎样判断一列数字都大于某一个值？
- 4. 用下列代码产生一列随机数，并求最大值、最小值与和。

```python
In [1]: import random
In [2]: [random.normalvariate(0,1) for i in range(20)]
```

## 第6章 Python常用语句

Python 程序由模块（Module）所构成，而模块中则包含各种语句（Statement）。藉由语句的组合，指定 Python 执行程序的过程。Python 语句可由表达式组成。表达式由运算符和操作数组组成，表达式会传回一个值；而语句是一个完整的句子，其不会返回一个值。Python 环境中也支持更复杂的语句，体现了不同语法与语意并对应不同的执行方式，下面我们将一一介绍。

### 6.1 赋值语句

#### 6.1.1 赋值含义与简单赋值

赋值语句通过赋值运算符、表达式和变量来实现。最常用的赋值运算符就是等号“=”，通过赋值符号“=”将其右边表达式的返回值赋给等号左边的变量。比如：

```python
a=3+4
```

在语句“a=3+4”中，“3+4”为表达式，该表达式运算出来的值通过赋值符号“=”赋给变量a。

Python 中的赋值过程，赋给变量的是内存中对象的引用，而不是对象的值。这句话怎么理解呢？请看下面的例子：

```python
In [1]: a=[1,2,3]

In [2]: id(a)
Out[2]: 159272200

In [3]: b=a

In [4]: id(b)
Out[4]: 159272200
```

赋值语句“a=[1,2,3]”使得变量 a 获得一个引用，指向一个列表对象 [1,2,3]。语句“b=a”表示变量 b 引用变量 a 所指向的对象，则变量 a 和变量 b 引用同一个对象，所以变量 a 和变量 b 的 id 相同。如图 6.1 所示，对这一原理进行了示意说明。

再执行下面的语句

```python
In [5]: b[1]=25
```

语句“b[1]=25”将 b 指向的列表对象的索引值为 1 的取值由 2 改成了 25，这一过程的示意图如图 6.2 所示。

![](img/0de14a685304f8aa267f6492145864ac_74_0.png)

![](img/0de14a685304f8aa267f6492145864ac_74_1.png)

从图 6.2 中可以看出，变量 a 和变量 b 依旧引用该列表对象，进而这两个变量的 id 没有发生变化。但此列表对象的值有所改变，所以变量 a 和变量 b 的取值发生了变化。下面通过代码来验证这一思想：

```python
In [6]: id(a)
Out[6]: 1592722200 # 变量 a 的 id 没有变化

In [7]: id(b)
Out[7]: 1592722200 # 变量 b 的 id 不变

In [8]: a
Out[8]: [1, 25, 3] # 变量 a 的取值改变

In [9]: b
Out[9]: [1, 25, 3] # 变量 b 的取值改变
```

再将变量 a 和变量 b 共同指向的对象的索引值 1 所对应的值改回 2，然后给变量 c 也赋值 [1, 2, 3]，语句如下：

```python
In [10]: b[1]=2

In [11]: a
Out[11]: [1, 2, 3]

In [12]: b
Out[12]: [1, 2, 3]

In [13]: c=[1,2,3]
```

查看变量 c 的 id 值：

```python
In [14]: id(c)
Out[14]: 159040712
```

从中可以看出，变量 c 与变量 a 的 id 值不一样，这又是怎么回事呢？如图 6.3 所示，变量 c 引用了另一个列表对象，该列表对象索引值 0、1 和 2 所引用的值也分别为 1、2 和 3。

![](img/0de14a685304f8aa267f6492145864ac_75_0.png)

很显然，就变量 c 和变量 a 来说，其指向的列表对象不同，进而 id 也不相同。然而，这两个列表对象的取值一样，则变量 c 和变量 a 的取值也一样。

```python
In [15]: c is a
Out[15]: False  # 变量 c 和变量 a 的 id 值不一样

In [16]: c==a   # 变量 c 和变量 a 的取值相同
Out[16]: True
```

按照相同的思路来分析，下面的代码结果就不足为怪了：

```python
In [17]: c[2]=88
In [18]: c
Out[18]: [1, 2, 88]

In [19]: a
Out[19]: [1, 2, 3]

In [20]: b
Out[20]: [1, 2, 3]
```

#### 6.1.2 多重整赋值

多重整赋值的含义是将同一个对象的引用赋值给多个变量，即多个变量同时指向该对象。

```python
In [21]: x=y=z=25678
In [22]: x
Out[22]: 25678

In [23]: y
Out[23]: 25678

In [24]: z
Out[24]: 25678

In [25]: x1=y1=z1=[2,5,6,7,8]

In [26]: x1
Out[26]: [2, 5, 6, 7, 8]

In [27]: (x1 is y1) and (y1 is z1) and (x1 is z1)
Out[27]: True
```

#### 6.1.3 多元赋值

多元赋值是指一个赋值语句实现了多个变量引用多个对象的赋值过程。举例如下：

```python
In [28]: x2, y2, z2=2, 5, 6

In [29]: x2
Out[29]: 2

In [30]: y2
Out[30]: 5

In [31]: z2
Out[31]: 6
```

赋值等号“=”左边为多个变量，变量之间用逗号隔开，右边为对象，对象也用逗号隔开。请注意，多重赋值和下面的单变量赋值进行区分：

```python
In [32]: x3=2, 5, 6
In [33]: x3
Out[33]: (2, 5, 6)
```

赋值等号“=”右边的表达式相同，都是用逗号隔开对象，但是等号左边只有一个变量x3，此时变量x3 的取值为元组类型数据。

#### 6.1.4 增强赋值

增强赋值的含义为对变量进行运算操作的结果再次赋给这个变量，比如 a=a*3 或者 a*=3。

```python
In [34]: a=6
In [35]: a*=3
In [36]: a
Out[36]: 18

In [37]: b=17
In [38]: b-=6

In [39]: b
Out[39]: 11

In [40]: c=21
In [41]: c%=3

In [42]: c
Out[42]: 0
```

### 6.2 条件语句

Python 条件语句是指通过对指定条件的真假结果来确定要执行哪条语句。Python 条件语句一般指 if 语句，if 条件语句的基本形式为：

```python
if condition1:
    statement1
else:
    statement2
```

在 if...else... 条件句中，if 条件判断语句所在行和 else 所在行都要以冒号结尾，而执行语句行要缩进。从上述基本形式中可以看出，condition1 后面有冒号，statement1 所在行有缩进，语句后面没有冒号。如果表达式 condition1 执行结果为 True，接下来执行 statement1 的代码；否则，执行 statement2 的代码。

```python
In [1]: a=6
         b=4
         if a>b:
             print('变量a的值大于变量b的值')
         else:
             print('变量b的值大于或等于变量a的值')

变量a的值大于变量b的值

In [2]: if a>5 and b>5:
         a+=6
         else:
             b+=6

In [3]: a
Out[3]: 6
In [4]: b
Out[4]: 10
```

如果判断条件有多个，则可以使用 if...elif 语句形式，如下：

```python
if condition1:
    statement1
elif condition2:
    statement2
else:
    statement3
```

代码示例为：

```python
In [5]: grade=95
         if grade>=90:
             print("Excellent job")
         elif all([grade>70, grade<90]):
             print("good job")
         else:
             print("It's uncommon")
Excellent job
```

Python 的条件语句还有更为简练的方式：所有代码都放置在一个物理行中即可完成条件语句程序。前面关于变量 a 和 b 大小条件判断的代码可以简化成如下形式：

```python
In [6]: a=6
In [7]: b=4
In [8]: '若 a 的值大于或等于 b 时 a 的值' if a>b else '若 a 的值小于 b 时 b 的值'
Out[8]:' a 的值大于或等于 b 的值'
```

这种条件语句简单的代码程序还可以用于赋值操作，即三元操作。三元操作的主要形式为：

```python
a=X if expression else Z
```

该三元操作语句的含义为：

- 当 expression 返回值为 True 时，执行语句 a=X；
- 当 expression 返回值为 False 时，执行语句 a=Z。

```python
In [9]: a=4*3 if {} else '123'
In [10]: a
Out[10]: '123'
```

空的字典数据返回值为 False，所以执行语句“a='123'”，进而变量 a 的取值为'123'。

### 6.3 循环语句

Python 有三种循环类型，分别为 for 循环、while 循环和内嵌循环。与这三种循环类型相关的循环控制语句有 break 语句、continue 语句和 pass 语句。下面分别介绍这些内容。

#### 6.3.1 for 循环

for 循环类型用于遍历一个序列对象（如列表、字符串等）中的所有元素。Python 中 for 循环的语法形式为：

```python
for variable in sequence:
    statements
```

此处的 statements 表示单个语句或者一个语句块。语句块由多个语句组成。for 循环语句或者语句块需要缩进，语句块内的语句的缩进格式相同。使用 for 循环时，要注意语句的缩进格式：

```python
In [1]: for i in [2,3,5,6,7]:
     ...:
     ...     print(i)
     ...
```

```python
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

如果语句“print(a)”没有缩进，则会出现不同的 print 结果：

```python
In [4]: a=list()
In [5]: for i in 'python':
...:        a.append(i+'python')
...: print(a)
...:
['ppython', 'pypython', 'tpython', 'hpython', 'opython', 'npython']
```

上述语句还可以写出较为简单的形式：

```python
In [6]: a=[i+'python' for i in 'python']
In [7]: a
Out[7]: ['ppython', 'pypython', 'tpython', 'hpython', 'opython', 'npython']
```

再举一例，如果要判断一个列表中 10 的索引值，可以使用如下代码：

```python
In [8]: list1=[2,10,34,3,10,20,10]
In [9]: [i for i in range(len(list1)) if list1[i]==10]
Out[9]: [1, 4, 6]
```

#### 6.3.2 while 循环

在 Python 中，while 循环语句的形式为：

```python
while expression:
  statements
```

当 while 循环的 expression 为 True 时，执行 statements 代表的语句或者语句块内容；当 expression 为 False 时，跳出循环，直接执行 statements 下面一行的代码。

```python
In [1]: a=0
In [2]: while a<4:
...:        a=a+1
...:        print(a+26)
...: print(a)
```
