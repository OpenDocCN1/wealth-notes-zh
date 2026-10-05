+   要恰当地运用 break 和 continue 语句，以便适时结束循环；

+   在 while 语句中还可以再写 while 语句，从而形成循环嵌套。

# 2.4.6 break 语句与 continue 语句

在 Python 中用于终止循环的语句有 break 语句和 continue 语句，这两种语句跳出循环的层次不同，但各有用处。

# 1. break 语句

break 语句是用来终止程序的执行的。当在循环结构中出现 break 语句时，即使在循环条件中没有 false 条件，或者序列还没被完全递归完，也会终止循环语句。如果 break 语句出现在嵌套循环中，则应当终止执行最深层的循环，并开始执行下一行代码。

break 语句在 while 循环中的示例如下：

```
In [57]: a=1
    while a:
        print a
        print "hello world"
        a=a+1
        if a==3:
            break

Out[57]:
    1
    hello world
    2
    hello world
```

break 语句在 for 循环中的示例如下：

```
In [58]: for i in range(5,9):
        print i
        print "hello world"
        if i>6:
            break

Out[58]:
    5
    hello world
```