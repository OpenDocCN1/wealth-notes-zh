使得断开重连时可以自动重新订阅。

其示例代码如下：

```
#---------------------------------------------------
def login(self, address, userid, password, brokerid):
    """连接服务器"""
    self.__userid = userid
    self.__password = password
    self.__brokerid = brokerid

    # 注册服务器地址
    self.registerFront(address)

    # 初始化连接成功后会调用 onFrontConnected
    self.init()

#---------------------------------------------------
def subscribe(self, instrumentid, exchangeid):
    """订阅合约"""
    req = {}
    req['InstrumentID'] = instrumentid
    req['ExchangeID'] = exchangeid
    self.subscribeMarketData(req)

    instrument = (instrumentid, exchangeid)
    self.__setSubscribed.add(instrument)
```

# 3.6 事件引擎

事件引擎是 vn.py 的核心组件，通过对接底层接口和上层应用模块，维持整个交易系统的正常运作。与常见的时间驱动不同，事件引擎的设计基于事件驱动。

# 3.6.1 时间驱动

时间驱动就是让计算机每隔一段时间固定运行一次脚本，脚本自身可以很长、包含非常多的步骤，但是这种程序的运行机制相对比较简单、容易理解。下面提