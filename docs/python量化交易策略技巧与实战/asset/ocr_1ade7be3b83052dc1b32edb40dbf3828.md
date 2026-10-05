函数内声明的变量名称都将被加入作用域中。

单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的“File”→“New File”命令，创建一个 Python 文件，并将其命名为“Python5-16.py”，然后输入如下代码：

```
# 这是一个全局变量
total = 0
def sum( arg1, arg2 ):
    # total 在这里是局部变量
    total = arg1 + arg2
    print ("函数内是局部变量：", total)
    print()
    return total;
#调用 sum 函数
mya = sum( 12, 38 )
print ("函数外是全局变量：", total)
print()
print("函数 sum 的返回值: ",mya)
```

在这里可以看到，首先定义一个全局变量 total，其值为 0。然后自定义函数 sum()，该函数有两个参数，并且都是必需参数。函数 sum()的功能是实现两个数相加，然后显示运算结果，并把运算结果返回。需要注意的是，函数内的变量 total 是局部变量，其值不会影响全局变量 total。

然后调用 sum()函数，并显示函数外的全局变量 total 及函数 sum 的返回值。

单击菜单栏中的“Run”→“Run Module”命令或键盘上的“F5”键，即可运行程序代码，结果如图 5.21 所示。

![](bbox=[0.235, 0.615, 0.788, 0.796])

# 5.4.3 global 和 nonlocal 关键字

当内部作用域想修改外部作用域的变量时，就要用到 global 和 nonlocal 关键字。下面先来看