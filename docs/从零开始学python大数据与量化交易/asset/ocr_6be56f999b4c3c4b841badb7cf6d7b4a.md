# 从零开始学 Python 大数据与量化交易

File 命令，创建一个 Python 文件，并命名为 Python5-9.py，然后输入以下代码：

```
#可写函数说明
def printinfo(name, sex, age, score):
    "打印任何传入的字符串或数值"
    print("名字:", name)
    print("性别: ", sex)
    print("年龄:", age)
    print("成绩: ", score)
    return
#调用 printinfo 函数
printinfo( age=18, name="张红", sex="男", score=89 )
```

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 5.12 所示。

# 3. 默认参数

调用函数时，如果没有传递参数，则会使用默认参数。

默认参数示例如下。

单击“开始”菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python5-10.py，然后输入以下代码：

```
def printinfo(name,score,age=13,sex='女'):
    print("名字:", name)
    print("性别: ", sex)
    print("年龄:", age)
    print("成绩: ", score)
    return
#第一次调用 printinfo 函数
printinfo( age=12, name="张水平",sex="男", score=97 )
print()
print("-------------------")
#第二次调用 printinfo 函数
printinfo( name="李路", score=85 );
```

需要注意的是，含有的默认参数要放在必需参数的后面；否则程序会报错。

第一次调用函数，用的是关键字参数；第二次调用函数用到了默认参数，即调用函数是没有传入 age 和 sex 的数值，这样就采用默认参数值。

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 5.13 所示。

![](bbox=[0.55, 0.315, 0.975, 0.5])