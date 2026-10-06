```
self.__weight = w
def speak(self):
    print("%s 说: 我 %d 岁。"%(self.name,self.age))
#单继承
class student(people):
    grade = "
    def __init__(self,n,a,w,g):
        #调用父类的构造函数
        people.__init__(self,n,a,w)
        self.grade = g
    #覆盖父类的方法
    def speak(self):
        print("%s 说: 我 %d 岁了，我在读 %d 年级"%(self.name,self.age,self.grade))
#另一个类，多重继承之前的准备
class speaker():
    topic = "
    name = "
    def __init__(self,n,t):
        self.name = n
        self.topic = t
    def speak(self):
        print("我叫 %s，我是一个演讲家，我演讲的主题是 %s"%(self.name,self.topic))
#多重继承
class sample(speaker,student):
    a ="
    def __init__(self,n,a,w,g,t):
        student.__init__(self,n,a,w,g)
        speaker.__init__(self,n,t)
test = sample("张亮",15,70,7,"Python")
test.speak()    #方法名同，默认调用的是在括号中排前父类的方法
```

在上述代码中，先定义了 3 个类，分别是 people 类、student 类、speaker 类，其中 student 类是 people 类的子类，需要注意的是，3 个类中都有 speak()方法。

然后，定义 sample 类，sample 类多重继承，即是 student 类的子数，同时也是 speaker 类的子数。sample 类定义了一个类变量，即 a；定义一个构造方法，在构造方法中调用了父类 student 的构造方法和父类 speaker 的构造方法。

最后，sample 类实例化，再调用 speak()方法。需要注意的是，由于 people 类、student 类、speaker 类中都有 speak()方法，在默认情况下，调用的是在括号中排前父类的方法，即 speaker 类中的 speak()方法。

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 6.4 所示。