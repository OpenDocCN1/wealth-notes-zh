# Python 与量化投资：从基础到实战

```
if i>7:
    print 9
    raise NameError
    print 10
Out[114]:
8
9
----------------------------
NameError                               Traceback (most recent call last)
<ipython-input-5-c740ebd6a5c9> in <module>()
  5 if i>7:
  6     print 9
----> 7     raise NameError
  8     print 10

NameError:
```

自定义一个异常类的实例如下：

```
In [115]: class SoSError(Exception):             #按照命名规范，以 Error 结尾，并且自定义异常需要继承 Exception 类
    def __init__(self):
        Exception.__init__(self)
try:
    i=8
    if i>7:
        raise SoSError()
except SoSError,a:
    print "SoSError:我错了"
Out[115]: SoSError:我错了
```

# 2.7.3 try...finally 的使用

无论是否发生异常，try...finally 语句都将执行最后的代码：

```
"""
#try...finally 语句的语法格式为：
try:
    #运行识别的代码
finally:
```