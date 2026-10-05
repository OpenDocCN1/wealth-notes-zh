# Python 与量化投资：从基础到实战

```
In [9]: #字符串的重复
    print "我爱 Python\n"*10
Out[9]: 我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
我爱 Python
```

字符串"wangxiaochuan"中的"wang"、"xiao"等都是"wangxiaochuan"的子字符串。如果想将一个字符串中的子字符串取出来，就要进行子字符串运算。而子字符串的运算方法主要有两种，一种是索引运算[]，另一种是切片运算[:]，示例如下：

```
In [10]: #子字符串
    #索引运算符从 0 开始索引
    #切片运算符[a:b]是指从第 a 下标开始到第 b-1 下标。
    c1=u"wangxiaochuan"
    c2=c1[0]
    c3=c1[7]
    c4=c1[:2]
    c5=c1[2:]
    c6=c1[4:7]
    c7=c1[-1]
    c8=c1[-3:]
    cuts=[2,4,6]
    c9=c1+c2
    print c2
    print c3, c4, c5, c6,c7,c8,c9
Out[10]: w
    o wa ngxiaochuan xia n uan wangxiaochuanw
```

注意，Python 是以 0 作为开始来计算位数的，所以 c1[1]是 “a” 而不是 “w”。“:” 代表从第几位取到第几位，如果 “：” 的前后都是数字，那么其作用是 “包前不包后”，c1[4:7] 代表取 c1 的[4,5,6]位置的元素，不包括第 7 位的元素。