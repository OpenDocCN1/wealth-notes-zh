```
```
Out[50]: 'girl'
peter.say()
Out[51]: 'i am a boy'
```
```

这里 gender 和 interest 是 peter 的属性，而 say 是 peter 的方法。如果再实例化另一个对象，例如 sam：

```
```
sam = boy()
sam.gender
Out[54]: 'male'
sam.interest
Out[55]: 'girl'
sam.say()
Out[56]: 'i am a boy'
```
```

那么 sam 和 peter 就有一样的属性和方法。

# ## 练习题

+   1. 用字典形式读入如下数据。

```
```
conc  state
0.02  treated
0.06  treated
0.11  treated
```
```

+   2. 编写一个函数，求数据 $y=(y_1, y_2, \cdots, y_n)$ 的均值、标准差。