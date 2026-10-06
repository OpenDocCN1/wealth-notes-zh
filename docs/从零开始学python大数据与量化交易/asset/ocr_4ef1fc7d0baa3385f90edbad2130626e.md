| 函数名称 | 功能说明 |
|---|---|
| seed([x]) | 改变随机数生成器的种子 seed。如果不了解其原理，不必特别去设定 seed，Python 会帮你选择 seed |
| shuffle(lst) | 将序列的所有元素随机排序 |
| uniform(x, y) | 随机生成下一个实数，它在[x, y]范围内 |

随机数函数示例如下。

单击“开始”菜单，打开 Python 3.6.5 Shell 软件，然后在菜单栏中选择 File→New File 命令，创建一个 Python 文件，并命名为 Python5-2.py，然后输入以下代码：

```
import random  #导入 random 模块
print("从 range(100) 返回一个随机数：",random.choice(range(100)))
print ("从列表中 [1, 2, 3, 5, 9]) 返回一个随机元素：", random.choice([1, 2, 3, 5, 9]))
print("从字符串中 'Runoob' 返回一个随机字符：", random.choice('Runoob'))
print()
# 从 1-100 中选取一个奇数
print("randrange(1, 100, 2):", random.randrange(1,100,2))
# 从 0-99 选取一个随机数
print("randrange(100):", random.randrange(100))
print()
# 第一个随机数
print("random():", random.random())
# 第二个随机数
print("random():", random.random())
print()
random.seed()
print()
print("使用默认种子生成随机数：", random.random())
random.seed(10)
print("使用整数种子生成随机数：",random.random())
random.seed("hello",2)
print("使用字符串种子生成随机数：", random.random())
print()
list = [20, 16, 10, 5];
random.shuffle(list)
print("随机排序列表：",list)
random.shuffle(list)
print ("随机排序列表 :",list)
print ("uniform(5, 10) 的随机浮点数 :",random.uniform(5, 10))
print ("uniform(7, 14) 的随机浮点数 :", random.uniform(7, 14))
```