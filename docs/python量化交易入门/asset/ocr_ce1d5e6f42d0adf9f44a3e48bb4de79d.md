```
event.dict_['log'] = log
self.__eventEngine.put(event)

# 重连后自动订阅之前已经订阅过的合约
if self.__setSubscribed:
    for instrument in self.__setSubscribed:
        self.subscribe(instrument[0], instrument[1])
....
#--------------------------------------------------------------------------------
def onRtnDepthMarketData(self, data):
    """行情推送"""
    # 行情推送收到后，同时触发常规行情事件，以及特定合约行情事件，用于满足不同类型的监听。

    # 常规行情事件
    event1 = Event(type_=EVENT_MARKETDATA)
    event1.dict_['data'] = data
    self.__eventEngine.put(event1)

    # 特定合约行情事件
    event2 = Event(type_=(EVENT_MARKETDATA_CONTRACT+data['InstrumentID']))
    event2.dict_['data'] = data
    self.__eventEngine.put(event2)
...
```

# 2. 主动函数

主动函数仅封装了两个功能：登录（login）和订阅合约（subscribe）。

对于登录函数而言，传入参数包括服务器前置机地址 address、用户名 userid、密码 password，以及经纪商代码 brokerid。函数调用后，先将 userid，password 和 brokerid 保存下来，然后注册服务器地址 registerFront，并初始化连接。连接完成后，onFrontConnected 回调函数会被自动调用，之后发生的操作请参考回调工作流程。

订阅合约方面，CTP API 在期货方只需要传入合约代码，但是证券类的接口需要同时传入合约的代码，以及合约所在的交易所（因为存在两个证券交易所相同代码的情况）。发送订阅请求后，将该订阅请求保存在 setSubscribed 集合中，