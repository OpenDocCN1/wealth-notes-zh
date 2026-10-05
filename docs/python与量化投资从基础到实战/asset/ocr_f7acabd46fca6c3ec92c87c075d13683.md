Python 与量化投资：从基础到实战

6
hello world
7
hello world

将 break 语句用在双层循环语句中，跳出最内层的循环的示例如下：

```
In [59]: a=5
    while a<=8:
        a=a+1
        for i in range(1,3):
            print a,i
            if i==2:
                break
```

Out[59]:

6 1
6 2
7 1
7 2
8 1
8 2
9 1
9 2

# 2. continue 语句

continue 语句的功能是强制停止循环中的这一次执行，直接跳到下一次执行。即跳过当前循环的剩余语句，继续进行下一轮循环。

continue 语句在 while 循环中的示例如下：

```
In [60]: n = 0
    while n < 10:
        n = n + 1
        if n % 2 == 0:  # 如果 n 是偶数，则执行 continue 语句
            continue      # continue 语句会直接继续下一轮循环，后续的 print() 语句不会执行
        print n
```

Out[60]:

1
3
5