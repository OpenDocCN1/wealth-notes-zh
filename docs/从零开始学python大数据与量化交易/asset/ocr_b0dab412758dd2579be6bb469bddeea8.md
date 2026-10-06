# 第 3 章 Python 的基本语法及流程控制

选择菜单栏中的 Run→Run Module 命令或按键盘上的 F5 键，就可以运行程序代码，结果如图 3.23 所示。

![](bbox=[0.24, 0.13, 0.73, 0.41])

图 3.23 range()函数示例的运行结果

# 3.6 其他语句

在 Python 的基本流程控制中，还常常使用其他 3 种语句，分别是 break 语句、continue 语句和 pass 语句。

# 3.6.1 break 语句

使用 break 语句可以使流程跳出 while 或 for 的本层循环，特别是在多层次循环结构中，利用 break 语句可以提前结束内层循环。

需要注意的是，如何从 for 或 while 循环中终止，任何对应的循环 else 块将不再执行。

break 语句示例如下。

单击 “开始” 菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python3-17.py，然后输入以下代码：

```
for letter in "mypython":    # 第一个实例
    if letter == 'h':
        break
    print('当前字母为 :',letter)
print()
var = 15    # 第二个实例
```