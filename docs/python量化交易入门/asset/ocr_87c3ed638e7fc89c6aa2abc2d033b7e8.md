```
a.classroom                        #访问公共属性,用点号.来访问对象的属性
a._Student__headmaster   #访问私有属性,用点号.来访问对象的属性
a.head()                      #园长名字
a.displayCount()              #学生人数
a.displayStudent()            #学生信息
a.displayClassroom()          #教室号
a.displayKindergarten()       #学校名
Out[2]
''分别输出下面信息''
101                                              #访问公共属性
高仓文太                                         #访问私有属性
The Kindergarten headmaster : 高仓文太           #园长名字
Total Student 1                                  #学生人数
Name : 野原新之助 , Age: 5                       #学生信息
Classroom is 102                                 #教室号
Kindergarten is 双叶幼儿园                       #学校名
```

继承类的构造方法有以下两种。

（1）经典类的写法： 父类名称.init(self,参数 1,参数 2,…)。

（2）新式类的写法： super(子类,self).init(参数 1,参数 2,…)。

在定义子类的构造函数时，只有先继承再构造，才能继承父类的属性。

子类的方法如果和父类的方法重名，子类就会覆盖掉父类，这就是继承 “多态”。调用方只管调用，不管细节，而新增一个子类时，只要确保新方法编写正确，而不用管原来的代码，这就是著名的 “开闭” 原则。

+   （1）对扩展开放： 允许新增子类；
（2）对修改封闭： 不需要修改依赖父类的函数。

# 2.6.3 定义团体子类

定义团体子类代码如下。

```
In[3]
class Group(Student):
```