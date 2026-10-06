# 从零开始学 Python 大数据与量化交易

```
s = student('张建利',12,58,6)
s.speak()
```

在上述代码中，先定义 people 类，类中有两个基本属性（name 和 age）和一私有属性（__weight），类中有两个方法，即类的构造方法和 speak()方法；在继承类（子类 student）中，定义一个属性（grade），定义两个方法，即子类的构造方法和子类的 speak()方法，这样在调用时就会覆盖父类（people）的 speak()方法。

最后就是子类 student 实例化，再调用子类的 speak()方法。

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 6.3 所示。

![](bbox=[0.265, 0.295, 0.79, 0.465])

# 图 6.3 类的继承示例的运行结果

Python 同样有限地支持多继承形式。多继承的类定义的语法格式如下：

```
class DerivedClassName(Base1, Base2, Base3):
    <statement-1>
    ...
    <statement-N>
```

多继承示例如下。

单击“开始”菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python6-4.py，然后输入以下代码：

```
#类定义
class people:
    #定义基本属性
    name = ""
    age = 0
    #定义私有属性,私有属性在类外部无法直接进行访问
    __weight = 0
    #定义构造方法
    def __init__(self,n,a,w):
        self.name = n
        self.age = a
```