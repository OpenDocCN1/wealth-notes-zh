# Python 量化交易

载的数据排序是从最新到最老。

```
In[2]
# 定义合约代码
Symbol = '510050'
Exchange = 'SSE' #上交所
vtSymbol = '.'.join([symbol, exchange]) #股票(合约代码+交易所代码)

# 下载历史数据
data = ts.get_hist_data(symbol, '2017-01-01', '2018-01-01')
data = data.sort_index()

print u'数据下载完成'
```

（3）创建 MongoDB 连接。在 MongoClient 传入参数，第一个是 MongoDB 所在服务器的默认 IP（本机地址），第二个是默认端口 27017。然后返回客户端下的数据库对象，在数据库中查找 vtSymbol。

```
#创建MongoDB链接
client = pymongo.MongoClient('localhost', 27017)      #默认 IP 和默认端口
collection = client[DAILY_DB_NAME][vtSymbol]          #数据库中查找vtSymbol
collection.ensure_index('datetime')                   #保证索引的唯一性
print u'MongoDB链接成功'
```

在 MongoDB 数据库中可以看见连接已经创建好了，如图 5-8 所示。

![](bbox=[0.209, 0.638, 0.83, 0.89])

图 5-8 MongoDB 连接创建成功