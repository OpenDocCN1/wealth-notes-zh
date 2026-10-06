# 6.1.2  类定义与类对象

在 Python 中，类定义的语法格式如下：

```
class ClassName:
    <statement-1>
    ...
    <statement-N>
```

在类实例化后，可以使用其属性。实际上，创建一个类之后，可以通过类名访问其属性。

类对象支持两种操作，分别是属性引用和实例化。属性引用和 Python 中所有的属性引用一样的标准语法：obj.name。类对象创建后，类命名空间中所有的命名都是有效属性名。

类定义与类对象示例如下。

单击“开始”菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python6-1.py，然后输入以下代码：

```
class MyClass:
    x = 16            #定义类变量
    y = "Python class"
    def myfun(self):    #定义类方法
        return "hello Python!"
a = MyClass()   # 实例化类
# 访问类的属性和方法
print("MyClass 类的属性 x 为: ", a.x)
print("MyClass 类的属性 y 为: ", a.y)
print("MyClass 类的方法 myfun 输出为: ", a.myfun())
```

在这里可以看到，首先定义类，类名 MyClass，该类有两个类变量和一个类方法，类变量分别为 x 和 y，类方法为 myfun()。接着是实例化类，这样就可以调用类的属性和方法，最后利用 print()函数来显示。

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 6.1 所示。

![](bbox=[0.28, 0.745, 0.695, 0.874])

图6.1 类定义与类对象示例的运行结果