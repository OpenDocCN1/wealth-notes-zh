```
In [15]: SSEC1['Open']
Out[15]: 3332.7
In [16]: SSEC1.get('Open')
Out[16]: 3332.7

In [17]: for key in SSEC1.keys():
...        if type(SSEC1[key])==float:
...            SSEC1[key]+=10000
...        print(key,':',SSEC1.get(key))
...
High : 103336.8
Close : 103336.3
Date : 02-Mar-2015
Open : 103332.7
Low : 103298.7
```

上述代码将 SSEC1 中四种价格取值都增加了 10000。若要更新字典对象中键的取值，也可以调用 update() 方法。

```
In [18]: SSEC1.update(Open=2332.7, High=2336.8, Low=2298.7, Close=2336.3)
In [19]: SSEC1
Out[19]:
{'Close': 2336.3,
'Date': '02-Mar-2015',
'High': 2336.8,
'Low': 2298.7,
'Open': 2332.7}
```

# 字典元素的增加、删除与复制

字典类型数据也允许增加新的元素。比如，在 SSEC1 中，加入一个新的项 index。

```
In [20]: SSEC1['index']='000001.SS'

In [21]: SSEC1
Out[21]:
{'Close': 2336.3,
'Date': '02-Mar-2015',
'High': 2336.8,
'Low': 2298.7,
'Open': 2332.7,
'index': '000001.SS'}
```

与增加项目相对应的是删除项目。用 del 语句可以删除特定的键及其相对应的值。如果调用 clear() 方法，则删除字典中的所有 内容，返回空字典。

```
In [22]: del SSEC1['Date']
In [23]: 'Date' in SSEC1
Out[23]: False
In [24]: SSEC1
Out[24]:
{'Close': 2336.3,
'High': 2336.8,
```