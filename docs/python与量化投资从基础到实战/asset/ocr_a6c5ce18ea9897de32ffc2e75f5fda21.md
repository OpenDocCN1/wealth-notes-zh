```
In [53]: char=u"你好世界"      #U 代表这是 Unicode 编码
    tem=("元组：你好","元组：世界")
    list=["列表：你好","列表：世界"]
    dict={"first":"字典：你好","second":"字典：世界"}
    for i in char:
        print i
    for i in tem:
        print i
    for i in list:
        print i
    for i in dict:
        print i
Out[53]:
    你
    好
    世
    界
    元组：你好
    元组：世界
    列表：你好
    列表：世界
    second
    first
```

for...in 语句的使用要点如下。

+   ○ 适当运用 range、xrange 函数。
○ 在 for 中还可以再写 for，形成循环嵌套。
○ 合理地运用 lambda 代替 for 循环，可以提高运行速度。

# 2.4.5 认识循环结构 while

在 Python 中 while 语句也可以用来控制一段语句的重复执行，即在某个条件下循环执行某段程序，以处理需要重复处理的相同任务。

while 语句的格式用法如下：

```
while 条件为真:
    循环执行该部分语句
```