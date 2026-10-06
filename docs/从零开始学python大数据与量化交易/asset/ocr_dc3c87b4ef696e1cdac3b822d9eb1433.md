# 4.1.5 列表的 4 个函数

列表的 4 个函数的名称及功能说明如表 4.1 所示。

# 表4.1 列表的函数名及功能说明

| 列表的函数名 | 函数的功能说明 |
| len(list) | 列表元素个数 |
| max(list) | 返回列表元素最大值 |
| min(list) | 返回列表元素最小值 |
| list(seq) | 将元组转换为列表 |

列表的函数示例如下。

单击 “开始” 菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python4-4.py，然后输入以下代码：

```
list1 = ["I","love","python"]
list2 = [100, 200, 300,400,500]
print("列表 1 中的元素 :", list1)
print("列表 2 中的元素 :", list2)
print()
print("list1 的最大值:", max(list1))
print("list2 的最大值:", max(list2))
print("list1 的最小值:", min(list1))
print("list2 的最小值:", min(list2))
print("list1 的元数个数:",len(list1))
print("list2 的元数个数:",len(list2))
print()
# id() 函数用于获取对象的内存地址
print("列表 1 中第一个值的内存地址值: ", id(list1[0]))
print("列表 1 中第二个值的内存地址值: ", id(list1[1]))
print("列表 1 中第三个值的内存地址值", id(list1[2]))
print()
aTuple = (123, 'Google', 'Runoob', 'Taobao')  #定义元组
list1 = list(aTuple)                         #把元组变成列表
print("把元组变成列表后，其中的元素 :", list1)
```

在这里用到了元组。需要注意的是，元组与列表是非常相似的，区别在于元组的元素值不能修改，元组是放在括号中，列表是放在中括号中。

在菜单栏中选择 Run→Run Module 命令或按键盘上的 F5 键，就/result/programs/code，就可以运行程序代码，结果如图 4.4 所示。