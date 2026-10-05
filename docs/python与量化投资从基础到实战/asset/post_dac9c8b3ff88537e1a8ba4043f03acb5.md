
## 3. 全局变量

在 Python 中，如果想让某些变量的作用域为全局，也就是作用于程序的所有区域，就要对这个变量进行全局声明，在声明后这个变量就成了全局变量。下面通过实例来讲解全局变量：

```
In [88]: def func3():
    global i
    i=7
a=func3()
print a
print i
Out[88]: None
7
```

函数 func3 和 func2 的区别在于，在 func3 中多了 global i 这一行代码，这行代码的作用是在函数中给全局变量赋值。在以上代码中，在运行 func3 后再打印 i，输出的结果不再是 2，而是 7，这说明在运行 func3 后改变了全局变量 i 的值。

注：若要在函数中给全局变量赋值，就需要用 global 关键字声明。

## 2.5.4 对函数的调用与返回值

### 1. 对函数的调用

在程序运行时，若想执行某个函数，就需要调用这个函数。若想调用一个函数，则在定义函数之后，直接输入并运行一遍这个函数名即可；若想传递实参给这个函数，则直接在括号里输入实参即可。比如一个函数被定义为 def func3()，则在调用它时直接输入 func3(参数)即可，其中的参数可以省略：

```
In [89]: i=7
def func3():
    i=1
func3()    #调用函数
print i
Out[89]: 7
```

注：因为定义的全局变量 i 的值为 7，所以最后打印的结果为 7。

### 2. 函数的返回值

在 Python 中有的函数是有返回值的，有的函数是没有返回值的。有返回值的函数可以返回一个值，也可以返回多个值，函数的返回值是通过 return 语句来实现的。

有一个返回值的情况如下：

```
In [90]: def test():
    i=7
    return i
    print test()
Out[90]: 7
```

有多个返回值的情况如下：

```
In [91]: def test2(i,j):
    k=i*j
    return (i,j,k)
    a=test2(4,5)
    print a
Out[91]: (4, 5, 20)
```

从如上所示的两个例子中可以看出，函数的返回值可以是一个或多个。

从如下所示的例子可以更清楚地看出，函数不但可以返回多个值，还可以将返回值直接赋给多个变量：

```
In [92]: def test2(i,j):
    k=i*j
    return i,j,k
    a,b,c = test2(4,5)
    print a,b,c
Out[92]: 4 5 20
```

## 2.5.5 文档字符串

在 Python 中可以定义很多函数，但是对于开发人员来说，如果遇到太多的函数而并不很了解各个函数的作用，就会感到很混乱。这时如果想了解各个函数的作用，就得从头分析，很浪费时间。针对这个问题，我们提供了两种解决方式：第 1 种是在开发时为每个函数写一个文档进行说明；第 2 种是在每个函数开头的地方加上一行说明性文字，我们将其称为文档字符串，这样，我们在编程时就可以很快明白这个函数的作用及对参数的设定

等，代码也会更加清晰。下面通过实例来讲解在 Python 中对文档字符串的使用：

```
In [93]: def d(i,j):
    """这个函数实现一个乘法运算。
    函数会返回一个乘法运算的结果。"""
    k=i*j
    return k
    help(d)
Out[93]: Help on function d in module __main__:
 d(i, j)
    这个函数实现一个乘法运算。
    函数会返回一个乘法运算的结果。
```

通过对文档字符串的说明，我们就可以明白这个函数的功能及所返回结果的属性。

## 2.6 Python 模块

## 2.6.1 认识 Python 模块

- 1. 什么是模块

函数是可以实现一项或多项功能的一段程序，模块便是对函数功能的扩展，是可以实现一项或多项功能的程序块。其实，函数是一段程序，模块是一项程序块，函数和模块都是用来实现功能的，但是模块的作用范围比函数要广，在模块里可以包含多个函数。

- 2. 如何导入模块

在 Python 的一段程序中要使用某个模块，就必须先导入指定的模块，只有导入了某个模块，才能使用该模块。下面通过实例来讲解如何导入模块。

（1）通过 import 语句导入整个模块：

```
import module1, module2...  # 建议一个 import 语句只导入一个模块
import module as module_mo # 别名（也就是自定义的模块名称空间）
```

代码示例如下：

```
In [94]: import pandas
In [95]: import pandas as pd
```

Python 与量化投资：从基础到实战

（2）通过 from-import 语句导入指定模块的部分属性或进行模糊导入，具体细节请参见 2.6.3 节：

```
from module import name1,name2....
```

代码示例如下：

```
In [96]: from pandas import Series
```

## 3. sys 模块

Python 自带一些模块，我们把这些模块叫作标准库模块。标准库模块是指一类模块，而不是特指某一种模块。在标准库模块中也有非常多的模块，有的与电子邮件相关，有的与操作系统相关，有的与系统功能相关，等等。我们把在标准库中与系统功能有关的这些模块叫作 sys 模块。下面通过实例来讲解 sys 模块的基本使用方法：

```
In [97]: import sys
In [98]: print sys.argv[0]
D:/anaconda3/Scripts/ipython-script.py
In [99]: sys.getdefaultencoding()
Out[99]: 'utf-8'
```

sys 模块的常见函数列表如表 2-3 所示。

## 表 2-3

| 函数 | 功能 |
|---|---|
| sys.argv | 实现从程序外部向程序传递参数 |
| sys.exit([arg]) | 实现在程序的中间退出，arg=0 为正常退出 |
| sys.getdefaultencoding() | 用于获取系统当前的编码，一般默认为 ascii |
| sys.setdefaultencoding() | 设置系统默认的编码，在执行 dir（sys）时不会看到这个方法。在解释器中执行不通过时，可以先执行 reload(sys)，再执行 setdefaultencoding('utf-8)，将系统的默认编码设置为 utf-8 |
| sys.getfilesystemencoding() | 用于获取文件系统使用的编码方式，在 Windows 下返回'mbcs'，在 Mac 下返回'utf-8' |
| sys.path | 用于获取指定的模块搜索路径的字符串集合，可以将写好的模块放在所得到的某个路径下，这样就可以在程序中 import 时正确找到 |
| sys.platform | 用于获取当前的操作系统平台 |
| sys.stdin、sys.stdout、sys.stderr | stdin、stdout 及 stderr 变量包含与标准 I/O 流对应的流对象。如果需要更好地控制输出，而 print 不能满足我们的要求，就可以使用这些变量 |

## 2.6.2 from...import 详解

### 1. 学会使用 from...import

若想在 Python 中导入一个模块，则可以使用 import，但是 import 导入了这个模块的全部功能，并没有指定具体模块的某个属性或方法。若不仅想导入一个模块，还想导入模块中对应的某个功能，则可以使用 from...import 语句。下面通过实例来了解 from...import 语句的用法：

```
In [100]: from pandas import DataFrame
    df1 = DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1':range(7)})
    df1.head(2)
Out[100]:
    data1 key
0   0   b
1   1   b
```

### 2. 学会使用 from...import *

在 Python 中使用 from...import 语句时，只能一次导入一个模块的一个功能，若想一次性导入这个模块的所有功能，也就是导入所有属性与方法，则可以使用 from...import* 语句。下面通过实例来了解 from ...import *语句的用法：

```
In [101]: from pandas import *
    df1 = DataFrame({'key': ['b', 'b', 'a', 'c', 'a', 'a', 'b'], 'data1': range(7)})
    df1.head(2)
Out[101]:
    data1 key
0   0   b
1   1   b
```

通过对比 import 与 from ...import ...这两种导入模块的方法，我们可以清楚地发现 import...as...比 from...import *更简洁、高效，因为 from...import *看似简单，但需要用户牢记模块中的具体函数，本书也不推荐使用这种方法。

## 2.6.3 认识__name__属性

### 1. 认识主模块

在 Python 函数中，如果一个函数调用其他函数完成一项功能，这个函数就叫作主函数；如果一个函数没有调用其他函数，这个函数就叫作非主函数。模块也是如此，如果一个模块是被直接使用的，没有被其他模块调用，这个模块就叫作主模块；如果一个模块被其他模块调用，这个模块就叫作非主模块。

### 2. 认识__name__属性

在 Python 中有主模块与非主模块之分，那么如何区分主模块与非主模块呢？答案是：如果一个模块的__name__属性的值是__main__，那么这个模块是主模块，否则就是非主模块。其实我们可以将__name__看作一个变量，这个变量是系统给出的，其功能是判断一个模块是否是主模块。

### 3. 对__name__属性的使用

首先，我们分别看看这个模块在不同场景下的__name__的值：

```
In [102]: print __name__
Out[102]: __main__
```

然后，看看__name__属性的常用情况：

```
In [103]: if __name__=="__main__":
    print "It's main"
else:
    print "It's not main"
Out[103]: It's main
```

## 2.6.4 自定义模块

Python 在安装时就自带的模块叫作系统自带模块，这些模块不需要用户自己定义和编写。而有些模块刚好与这种模块不同，需要我们自己定义和编写，这些模块就叫作自定义模块。下面通过实例进行讲解：

```
def add(i,j):
    k=i+j
    return k
```

将这段语句保存在 myadd.py 文件中，并保存在“....\Anaconda\Lib”目录下就成为模块。注意：“....”为我们安装时的 Anaconda 的地址。

下面调用自定义模块 myadd:

```
In [104]: import myadd as md    #调用自定义模块 myadd
In [105]: s = md.add(1,3)
In [106]: s
Out[106]: 4
```

## 2.6.5 dir()函数

- 1. 认识 dir()函数

在 Python 中有很多模块，有时我们会忘记一个模块有哪些功能，这时可以通过 dir() 函数来查看指定模块的功能列表。下面通过实例进行讲解：

```
In [107]: import pandas
In [108]: dir(pandas)
Out[108]:
    ['Categorical', 'CategoricalIndex', 'DataFrame', 'DateOffset',
'DatetimeIndex', 'ExcelFile', 'ExcelWriter', 'Expr', 'Float64Index', 'Grouper',
'HDFStore', 'Index', 'IndexSlice', 'Int64Index', 'MultiIndex', 'NaT', 'Panel',
'Panel4D', 'Period', 'PeriodIndex', 'RangeIndex', 'Series', 'SparseArray',
'SparseDataFrame', 'SparseList', 'SparseSeries', 'SparseTimeSeries', 'Term',
'TimeGrouper', 'TimeSeries', 'Timedelta', 'TimedeltaIndex', 'Timestamp',
'WidePanel', '__builtins__', '__cached__', '__doc__', '__docformat__',
'__file__', '__loader__', '__name__', '__package__', '__path__', '__spec__',
'__version__', '_join', '_np_version_under1p10', '_np_version_under1p11',
'_np_version_under1p12', '_np_version_under1p8', '_np_version_under1p9', ...]
```

注意：鉴于 Pandas 的方法太多，这里并没有全部列出来。

- 2. dir()函数扩展详解

在 Python 中，通过 dir()函数不仅能查看模块的功能列表，还能查看任意指定对象的

功能列表。如下所示为定义 a 为一个列表，并通过 dir(a)查看 a 的功能列表：

```
In [109]: a = [1]
    dir(a)
Out[109]:
    ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__init_subclass__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'clear', 'copy', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

## 2.7 Python 异常处理与文件操作

## 2.7.1 Python 异常处理

异常指一个事件，该事件会在程序执行的过程中发生，从而影响程序的正常执行，我们将这种状态称为 Python 异常。在 Python 中一切皆对象，异常也是 Python 的对象，表示一个错误。当 Python 脚本发生异常时，我们需要捕获并处理它，否则程序会终止执行。我们通常会把异常的类型和位置打印出来，以便程序员处理：

```
In [110]: print 字符串需要被引用
Out[110]: File "<ipython-input-3-79d7652315a8>", line 1
    print 字符串需要被引用
    ^
SyntaxError: invalid syntax
```

在上述实例中，由于 Python 打印字符串时需要引用内容，所以会报错，解决方法为在输出内容前加 “”，代码如下：

```
In [111]: print "字符串需要被引用"
Out[111]: 字符串需要被引用
```

假如我们在编程时知道某些语句可能会导致某种错误的发生，如果不想在异常发生时结束程序，则只需在 try 里捕获它，然后使用 except 语句处理可能出错的部分。

对 Python 异常的处理如下：

```
""":
    try...except...else 的语法格式为:
    try:
        #运行识别的代码
    except:
        #如果 try 部分引发了异常，则执行此处的代码
    else:
        #如果都没有发生异常
"""
```

下面通过实例来讲解 try...except...else 的用法：

```
In [112]: #i=1
    try:
        print i
    except :      #这里一定要指明异常的类型
        i=9
        i+=10
        print "刚才 i 没定义，处理了异常之后，i 的值为: "+str(i)
Out[112]: 刚才 i 没定义，处理了异常之后，i 的值为: 19
```

当 i=1 为备注状态时，会执行 except 中的代码：

```
In [113]: #处理多种异常
    i=10
    #j="a"
    try:
        print i+j
    except NameError:
        i=j=0
        print "刚刚 i 或 j 没有进行初始化数据，现在我们将assic​初始化为 0，结果是: "
        print i+j
    except TypeError:
        print "刚刚 i 与 j 类型对应不上，我们转换一下类型即可处理异常，处理后：结果是: "+str(i)+str(j)
Out[113]:
    刚刚 i 或 j 没有进行初始化数据，现在我们将其都初始化为 0，结果是:
    0
```

常见的 Python 异常如表 2-4 所示。
