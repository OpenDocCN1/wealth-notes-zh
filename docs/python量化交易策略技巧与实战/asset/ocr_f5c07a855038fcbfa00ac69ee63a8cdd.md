```
a = set('abracadabra')
b = set()
```

集合的两个基本功能分别是去重和成员测试。去重，是指把一个含有重复元素的列表或元组等数据类型转变成集合，其中的重复元素只出现一次。成员测试，是指判断元素是否在集合内。

单击“开始”菜单按钮，打开“Python 3.7.1 Shell”窗口，然后单击菜单栏中的“File”→“New File”命令，创建一个 Python 文件，并将其命名为“Python4-18.py”，然后输入如下代码：

```
student = {'Tom', 'Jim', 'Mary', 'Tom', 'Jack', 'Rose'}
print("输出集合，重复的元素被自动去掉：",student)
# 成员测试
if('Rose' in student):
    print('Rose 在集合中')
else:
    print('Rose 不在集合中')
if('Zhoudiao' in student):
    print('Zhoudiao 在集合中')
else:
    print('Zhoudiao 不在集合中')
```

单击菜单栏中的“Run”→“Run Module”命令或按键盘上的“F5”键，即可运行程序代码，结果如图 4.18 所示。

![](bbox=[0.26, 0.55, 0.73, 0.72])

图 4.18 去重和成员测试

集合的运算符及意义如表 4.6 所示。

# 表 4.6 集合的运算符及意义

| 数学符号 | Python 符号 | 说明 |
|---|---|---|
| ∩ | & | 交集，如 a&b |
| ∪ | | | 并集，如 a|b |