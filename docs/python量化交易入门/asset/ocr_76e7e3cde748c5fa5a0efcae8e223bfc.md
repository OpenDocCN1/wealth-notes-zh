# （2）遍历字典里的元素，把股票代码全部改成大写。

```
# 2.对字典进行循环
'''
定义字典： 6 家公司(GAFATA) 的股票
key 是公司名称, value 是公司代码
'''
gafataDict = {'腾讯':'HK:00700','阿里巴巴':'baba','苹果':'Apple','谷歌':'GOOGLE','Facebook':'fb','亚马逊':'amzn'}
# 将股票代码全部改成大写(upper)
# 注意用 key, value
for key,value in gafataDict.items():
    newValue = value.upper()
    gafataDict[key]= newValue
print (gafataDict)
>> {'腾讯':'HK:00700','阿里巴巴':'BABA','苹果':'APPLE','谷歌':'GOOGLE','Facebook':'FB','亚马逊':'AMZN'}
```

# （3）用 continue 跳出当前循环。

```
# 3.continue 用于跳出当前循环
'''
定义字典： 6 家公司(GAFATA) 的股票
key 是公司名称, value 是公司代码
'''
gafataDict = {'腾讯':'HK:00700','阿里巴巴':'baba','苹果':'Apple','谷歌':'GOOGLE','Facebook':'fb','亚马逊':'amzn'}
# 注意用 key, value
for key,value in gafataDict.items():
    if key == '苹果':
        continue
print ('当前公司',key,'当前股票代码: ',value)
>> 当前公司 腾讯 当前股票代码: HK:00700
当前公司 阿里巴巴 当前股票代码: baba
当前公司 谷歌 当前股票代码: GOOGLE
当前公司 Facebook 当前股票代码: fb
当前公司 亚马逊 当前股票代码: amzn
```

# （4）用 break 退出整个循环。