```
In[1]
# encoding: UTF-8
class Student(object):
    """定义学生父类"""

    #类的属性
    studCount = 0              #初始化学生人数，以便后面统计
    classroom = '102'          #教室号
    kindergarten = '双叶幼儿园' #地点
    name = None                #学生名字
    age = 0                    #学生年龄

    #实例方法------------------------------------------------------------
    def __init__(self, name, age):
        """类的初始化"""
        self.name = name        #学生名字
        self.age = age          #学生年龄
        self.studCount += 1     #用于统计学生人数
        self.__headmaster = '高仓文太' #私有属性

    def head(self):
        """园长"""
        print 'The Kindergarten headmaster : ',self.__headmaster

    def displayCount(self):
        """学生人数统计"""
        print "Total Student %d" % self.studCount

    def displayStudent(self):
        """学生信息"""
        print "Name : ", self.name, ", Age: ", self.age

    #类的方法------------------------------------------------------------
    @classmethod
    def displayClassroom(cls):
        """教室号"""
        print "Classroom is %s" % cls.classroom

    #静态方法------------------------------------------------------------
    @staticmethod
    def displayKindergarten():
```