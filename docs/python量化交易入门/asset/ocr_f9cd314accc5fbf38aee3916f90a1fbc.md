```
>> True
# 5修改集合内的元素
#第一步，删除
BitcoinExchangeSets.discard('火币比特币交易所')
#第二步，增加
BitcoinExchangeSets.update(['OKEX 比特币交易所'])
print (BitcoinExchangeSets)
>> {'LBANK 比特币交易所', "OKEX 比特币交易所"}
```

# 4. 字典

字典是另一种可变容器模型，可存储任意类型的对象。字典的每个键值对 “key=>value” 用冒号 “:” 分隔，每个对之间用逗号 “,” 分隔，整个字典包括在花括号 “{ }” 中，格式为 d = {key1 : value1, key2 : value2 }。键必须是唯一的，且是不可变的，但值则不必需唯一，可取任何数据类型，如字符串、数字或元组。字典的基本操作举例说明如下。

（1）创建新的字典。

```
# 创建新的字典
ExchangeDict = {'中金所':'CFFEX',
    '上期所':'SHFE',
    '郑商所':'CZCE',
    '大商所':'DCE',
    '国际能源交易中心':'INE'}
```

（2）增加、删除、查询和修改元素。

```
# 1. 增加元素
ExchangeDict['上金所']=['SGE']
ExchangeDict
>> {'上期所': 'SHFE',
'上金所': ['SGE'],
'中金所': 'CFFEX',
'国际能源交易中心': 'INE',
'大商所': 'DCE',
'郑商所': 'CZCE'}
```