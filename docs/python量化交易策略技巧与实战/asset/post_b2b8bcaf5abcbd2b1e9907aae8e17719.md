
```
print ("名字: ", name)
print ("性别: ", sex)
print ("年龄: ", age)
print ("成绩: ", score)
    return
#第一次调用 printinfo 函数
printinfo( age=12, name="张永平",sex="男", score=97 )
print()
print ("-----------------------------")
#第二次调用 printinfo 函数
printinfo( name="李路", score=85 )
```

需要注意的是，含有默认参数要放在必需参数的后面，否则程序会报错。

第一次调用函数，用到了关键字参数；第二次调用函数用到了默认参数，即调用函数时没有传入 age 和 sex 的数值，于是采用了默认参数值。

单击菜单栏中的 “Run” → “Run Module” 命令或按键盘上的 “F5” 键，即可运行程序代码，结果如图 5.13 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_138_0.png)

## 4）不定长参数

有时可能需要一个函数能处理比当初声明时更多的参数，这些参数叫作不定长参数，和上述三种参数不同，该参数声明时不会命名，基本语法如下：

```
def functionname([formal_args,] *var_args_tuple):
    function_suite
    return [expression]
```

加了星号（*）的变量名会存放所有未命名的变量参数，如果在函数调用时没有指定参数，它就是一个空元组。

单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的“File”→“New File”命令，创建一个 Python 文件，并将其命名为“Python5-11.py”，然后输入如下代码：

```
def printinfo(arg1, *vartuple):
    "打印任何传入的参数"
    print ("必需参数的值: ",arg1)
    print()
    if len(vartuple)==0:
        print("没有可变参数传入")
    else:
        for var in vartuple:
            print ("可变参数的值: ",var)
    return
#第一次调用 printinfo 函数
printinfo( 80 );
print ("------------------------")
#第二次调用 printinfo 函数
printinfo( 120, 110, 40,90,50 )
```

该代码两次调用函数，第一次调用函数时只传入必需参数，这样可变参数的变量的长度就为 0，即 len(vartuple)==0 成立，这样将显示“没有可变参数传入”。

第二次调用函数时传入 5 个参数，第一个参数是必需参数，其他 4 个参数是可变参数。这样 len(vartuple)==0 不成立，程序运行 else 语句内容，即通过 for 循环语句显示可变参数的值。

单击菜单栏中的“Run”→“Run Module”命令或按键盘上的“F5”键，即可运行程序代码，结果如图 5.14 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_139_0.png)

图 5.14 Python5-11.py 运行结果

## 5.2.5 匿名函数

所谓匿名，就是不再使用 def 语句的形式定义一个函数。在 Python 中，使用 lambda 来创建匿名函数。匿名函数需要注意以下几点：

第一，lambda 只是一个表达式，函数体比 def 简单很多。

第二，lambda 的主体是一个表达式，而不是一个代码块，lambda 表达式只能封装有限的逻辑。

第三，lambda 函数拥有自己的命名空间，且不能访问自己参数列表之外或全局命名空间里的参数。

第四，虽然 lambda 函数看起来只能写一行，却不等同于 C 语言或 C++语言的内联函数，C 语言或 C++语言的目的是调用小函数时不占用栈内存从而增加运行效率。

lambda 函数的语法只包含一个语句，具体如下：

```
lambda [arg1 ,arg2,....argn]:expression
```

单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的 “File” → “New File” 命令，创建一个 Python 文件，并将其命名为 “Python5-12.py”，然后输入如下代码：

```
mylamb = lambda arg1, arg2, arg3 : arg1 + arg2 - arg3
# 调用匿名函数 mylamb
print ("调用匿名函数，并返回运算值：", mylamb( 30, 90,66 ))
```

单击菜单栏中的 “Run” → “Run Module” 命令或按键盘上的 “F5” 键，即可运行程序代码，结果如图 5.15 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_140_0.png)

图 5.15 Python5-12.py 运行结果

## 5.3 Python 的面向对象

Python 在设计之初就已经是一门面向对象的语言，所以在 Python 中创建一个类和对象很容易。

### 5.3.1 面向对象概念

面向对象概念主要有 9 个，具体如下。

### 1）类

类（Class）是用来描述具有相同的属性和方法的对象的集合。它定义了该集合中所有对象所共有的属性和方法，对象是类的实例。

### 2）类变量

类变量在整个实例化的对象中是公用的。类变量定义在类中且在函数体之外。类变量通常不作为实例变量使用。

### 3）数据成员

数据成员，即类变量或者实例变量，是用于处理类及其实例对象的相关数据。

### 4）方法重写

如果子类从父类继承的方法不能满足子类的需求，可以对其进行改写，这个过程叫方法的覆盖（override），也称为方法的重写。

### 5）实例变量

实例变量是定义在方法中的变量，只作用于当前实例的类。

### 6）继承

继承，即一个派生类（derived class）继承父类的字段和方法。继承也允许把一个派生类的对象作为一个父类对象对待。

### 7）实例化

实例化，即创建一个类的实例，类的具体对象。

### 8）方法

方法，即类中定义的函数。

### 9）对象

对象，即通过类定义的数据结构实例。对象包括两个数据成员（类变量和实例变量）和方法。

### 5.3.2 类与实例

在 Python 中，类定义的语法具体如下：

```
class ClassName:
    <statement-1>
    .
    .
    .
    <statement-N>
```

类实例化后，可以使用其属性。实际上，创建一个类之后，可以通过类名访问其属性。

类对象支持两种操作，分别是属性引用和实例化。属性引用和 Python 中所有的属性引用有一样的标准语法：obj.name。类对象创建后，类命名空间中所有的命名都是有效属性名。

很多类都倾向于将对象创建为有初始状态的。因此类可能会采用定义一个名为 __init__() 的特殊方法（构造方法），具体代码如下：

```
def __init__(self):
    self.data = []
```

如果类定义了 __init__() 方法，那么类的实例化操作会自动调用 __init__() 方法。当然，__init__() 方法可以有参数，参数可以通过 __init__() 方法传递到类的实例化操作上。

Python 同样支持类的继承，如果一种语言不支持继承，那么类就没有意义。派生类定义的语法具体如下：

```
class DerivedClassName(BaseClassName1):
    <statement-1>
    .
    .
    .
    <statement-N>
```

需要注意圆括号中父类的顺序，若是父类中有相同的方法名，而在子类使用时未指定，Python 需要从左至右搜索，即方法在子类中未找到时，Python 需要从左到右查找父类中是否包含方法。另外，父类必须与派生类定义在一个作用域内。

Python 同样支持有限的多继承形式。多继承的类定义的语法具体如下：

```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    .
    .
    .
    <statement-N>
```

单击 “开始” 菜单按钮，打开 “Python 3.7.1 Shell” 窗口，然后单击菜单栏中的 “File” → “New File” 命令，创建一个 Python 文件，并将其命名为 “Python5-13.py”，然后输入如下代码：

```
#类定义
class people:
```
```
#定义基本属性
name = ''
age = 0
#定义私有属性,私有属性在类外部无法直接进行访问
__weight = 0
#定义构造方法
def __init__(self,n,a,w):
    self.name = n
    self.age = a
    self.__weight = w
def speak(self):
    print("%s 说: 我 %d 岁。" %(self.name,self.age))
#单继承
class student(people):
    grade = ''
    def __init__(self,n,a,w,g):
        #调用父类的构造
        people.__init__(self,n,a,w)
        self.grade = g
    #覆写父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
#另一个类,多重继承之前的准备
class speaker():
    topic = ''
    name = ''
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演说家，我演讲的主题是 %s"%(self.name,self.topic))
#多重继承
class sample(speaker,student):
    a =''
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
test = sample("张亮",15,70,7,"Python")
test.speak()  #方法名同，默认调用的是在括号中排在前面的父类的方法
```

在上述代码中，先定义了 3 个类，分别是 people 类、student 类、speaker 类，其中 student 类是 people 类的子类，需要注意的是，3 个类中都有 speak()方法。

然后定义 sample 类,sample 类有多重继承,既是 student 类的子类,同时也是 speaker 类的子类。sample 类定义了一个类变量，即 a；定义了一个构造方法，在构造方法中调用了父类 student 的构造方法和父类 speaker 的构造函数。

最后，sample 类实例化，再调用 speak()方法。需要注意的是，由于 people 类、student 类、speaker 类中都有 speak()方法，默认情况下，调用的是在括号中排在前面的父类的方法，即 speaker 类中的 speak()方法。

单击菜单栏中的 “Run” → “Run Module” 命令或按键盘上的 “F5” 键，即可运行程序代码，结果如图 5.16 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_144_0.png)

### 5.3.3 模块的引用

模块是一个包含所有定义的函数和变量的文件，其后缀名是.py。模块可以被别的程序引入，以使用该模块中的函数等功能。

想使用 Python 源文件，只需在另一个源文件里执行 import 语句即可，其语法具体如下：

```
import module1[, module2[, ... moduleN]]
```

提醒：不管执行多少次 import，一个模块只会被导入一次。这样可以防止导入的模块被一遍又一遍地执行。

在 Python 中，用 import 或 from...import 来导入相应的模块，具体如下。

第一，将整个模块（somemodule）导入，格式为：import somemodule。

第二，从某个模块中导入某个函数，格式为：from somemodule import somefunction。

第三，从某个模块中导入多个函数，格式为：from somemodule import firstfunc, secondfunc, thirdfunc。

第四，将某个模块中的全部函数导入，格式为：from somemodule import *。

### 5.3.4 包

包是一种管理 Python 模块命名空间的形式，采用“点模块名称”。例如，一个模块的名称是 A.B，那么它表示一个包 A 中的子模块 B。

> 提示：包类似于文件夹，而模块类似于文件夹中的文件。

就像使用模块的时候，不用担心不同模块之间的全局变量相互影响一样，采用点模块名称这种形式也不用担心不同库之间模块重名的情况。这样不同的程序员就可以提供 Numpy 模块，或者是 Python 图形库。

假设设计一套统一处理声音文件和数据的模块（或者称 ，因为现在有好几种不同的音频文件格式，所以需要有一组不断增加的模块，用来在不同的格式之间转换。并且针对这些音频数据，还有很多不同的操作（比如混音、添加回声、增加均衡器功能、创建人造立体声效果），所以还需要编写相关的模块来处理这些操作。

这里给出了一种可能的包结构（在分层的文件系统中），具体如下：

```
sound/
    __init__.py                顶层包
                                初始化 sound 包
    formats/                   文件格式转换子包
        __init__.py
        wavread.py
        wavwrite.py
        aiffread.py
        aiffwrite.py
        auread.py
        auwrite.py
        ...
    effects/                    声音效果子包
        __init__.py
        echo.py
        surround.py
        reverse.py
        ...
    filters/                    filters 子包
        __init__.py
        equalizer.py
        vocoder.py
        karaoke.py
        ...
```

导入一个包的时候，Python 会根据 sys.path 中的目录来寻找这个包中包含的子目录。

只有目录包含一个叫作 __init__.py 的文件时，才会被认作是一个包，这主要是为了避免一些系统禁用的名字（比如 string）不小心影响到搜索路径中的有效模块。

打开 Python 当前工作目录，然后双击“Lib”，这里的子文件夹都是一个包，在这里双击“email”，在该文件夹下可以看到 __init__.py 文件，如图 5.17 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_146_0.png)

# 图 5.17 Python 中的包

打开__init__.py 文件，即可看到该文件中的代码，如图 5.18 所示。

![](img/e6d4c365781f4dd8cb814bd56923b0bc_146_1.png)

# 图 5.18 __init__.py 文件代码

## 5.4 变量作用域及类型

在 Python 程序设计中，程序的变量并不是在哪个位置都可以被访问到的，访问权限取决于这个变量是在哪里被赋值的。

### 5.4.1 变量作用域

变量的作用域决定了在哪一部分程序可以访问哪一个特定的变量名称。Python 的变量作用域一共有 4 种，分别是局部作用域（local）、闭包函数外的函数作用域（enclosing）、全局作用域（global）、内建作用域（built-in）。

在 Python 程序设计中，变量以局部作用域、闭包函数外的函数作用域、全局作用域、内建作用域的规则查找，即在局部找不到，便会去局部外的局部找（例如闭包），再找不到将去全局找，再者去内建中找，具体代码如下：

```
x = int(2.9)          # 内建作用域
g_count = 0           # 全局作用域
def outer():
    o_count = 1       # 闭包函数外的函数作用域
    def inner():
        i_count = 2   # 局部作用域
```

在 Python 程序设计中，只有模块、类和函数（如 def、lambda）才会引入新的作用域，其他的代码块（如 if/elif/else、for/while 等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问。

单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的“File”→“New File”命令，创建一个 Python 文件，并将其命名为“Python5-14.py”，然后输入如下代码：

```
if True:
    msg1="我是 if 语句中的变量，外部是可以访问的！"
else:
    msg2="我也是一个代码块变量"
print(msg1)  # 可以直接调用 if 语句中的变量
```

由于 if 语句的条件成立，所以 msg1 已指向“我是 if 语句中的变量，外部是可以访问的！”，由于这个变量已存在，所以可以直接利用 print()函数输出。需要注意的是，由于 if 语句的条件成立，就不会运行 else 中的语句，所以 msg2 没有指向“我也是一个代码块变量”，即不能利用 print()函数输出 msg2 变量。

单击菜单栏中的“Run”→“Run Module”命令或按键盘上的“F5”键，即可运行程序代码，
