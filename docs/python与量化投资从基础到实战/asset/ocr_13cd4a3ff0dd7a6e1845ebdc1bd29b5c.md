```
#退出时总会执行的语句
'''
```

下面通过实例讲解对 try...finally 语句的使用：

```
In [116]: #假如要实现不管中间是否发生异常，都要输出 hello world
    try:
        print i
    finally:
        print "不管上面是否异常，我必须输出 hello world! "
Out[116]:
    不管上面是否异常，我必须输出 hello world!
    ------------------------------------------------------------
    NameError                                Traceback (most recent call last)
    <ipython-input-10-88d42e4abf8f> in <module>()
      2 #假如要实现不管中间是否发生异常，都要输出 hello world
      3 try:
    ----> 4     print i
      5 finally:
      6     print "不管上面是否异常，我必须输出 hello world! "
    NameError: name 'i' is not defined
```

# 2.7.4 文件操作

我们可以 στιγ Python 程序直接自动操作某文件，例如：创建文件、打开文件、关闭文件、将指定的内容写入文件、读取文件、关闭文件等。

下面通过实例来讲解在 Python 中对文件的操作：

```
In [117]: #创建某个文件
    import os
    os.mkdir(r"d:/newdir")
```

下面通过实例来说明如何对一个文件进行写入和关闭文件。

第 1 步，做好内容：

```
In [118]: content='''我是文件内容
内容是
如何做好 Python 量化投资
'''
```