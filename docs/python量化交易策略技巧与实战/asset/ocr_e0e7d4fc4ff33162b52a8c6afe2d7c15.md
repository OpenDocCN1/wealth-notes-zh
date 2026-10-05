单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的“File”→“New File”命令，创建一个 Python 文件，并将其命名为“Python4-2.py”，然后输入如下代码：

```
a = 37
b = 12
if ( a and b ):
    print ("1:变量 a 和 b 都为 true")
else:
    print ("1:变量 a 和 b 有一个不为 true")
if ( a or b ):
    print ("2:变量 a 和 b 都为 true, 或其中一个变量为 true")
else:
    print ("2:变量 a 和 b 都不为 true")
a = 0  # 修改变量 a 的值
if ( a and b ):
    print ("3:变量 a 和 b 都为 true")
else:
    print ("3:变量 a 和 b 有一个不为 true")
if ( a or b ):
    print ("4:变量 a 和 b 都为 true, 或其中一个变量为 true")
else:
    print ("4:变量 a 和 b 都不为 true")
if not( a and b ):
    print ("5:变量 a 和 b 都为 false, 或其中一个变量为 false")
else:
    print ("5:变量 a 和 b 都为 true")
```

单击菜单栏中的“Run”→“Run Module”命令或按键盘上的“F5”键，即可运行程序代码，结果如图 4.2 所示。

![](bbox=[0.3, 0.635, 0.745, 0.835])

# 4.1.3 if 语句

Python 中 if 语句的一般形式如下：