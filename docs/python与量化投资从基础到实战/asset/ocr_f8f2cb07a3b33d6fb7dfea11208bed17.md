对 Python 异常的处理如下：

```
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