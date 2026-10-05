
![](img/111072991fcea5fcc2790f3fbbeb60af_149_0.png)

图 3-23 行情接口的工作原理

## 3.5.2 CTP API 的 Python 封装设计

### 1. Python 封装的必要性

基于 C++ 的 CTP API 只有先进行 Python 封装，才能直接被 vn.py 调用。在 ThostFtdcUserApiStruct.h 中定义的结构体，既无法在 Python 环境中直接创建，也无法提取结构体中包含的数据（回调函数）。Python 虚拟机是基于 C 语言实现的，Python 的对象在 C 环境中对应的是 PyObject 对象，故 C++ 也无法识别 Python 直接传递的参数。

### 2. 封装设计思路

原生 API 中的每个功能分为了两个类，分别是：包含回调函数的 Spi 类和包含主动函数的 Api 类，这种设计能让用户更好地分清不同的功能。但是从面向对象编程的角度出发，把两个类封装到一起更为方便。因此，在 Python 的 API 中，会把 Spi 和 Api 两个类的功能封装到一个类中。

原生 API 中的回调函数被触发后必须快速返回，否则会导致其他数据的推送被阻塞，阻塞时间长了还有可能导致 API 崩溃，因此回调函数中不适合包含耗时较长的计算逻辑。例如，某个 Tick 行情推送后，如果用户在回调函数中写了一些比较复杂的计算，如循环计算，耗时超过 3 秒钟，则在这个 3 秒钟中，用户是收不到其他行情推送的，且很可能 3 秒钟后会出现 API 崩溃。这里的解决方案是使用生产者-消费者模型，在 API 中包含一个缓冲队列，当回调函数收到新的数据信息时只是简单存入缓冲队列中并立即返回，而数据信息的处理及向 Python 中的推送则由另一个工作线程来执行。

基于 C++的 CTP API 的函数中使用了大量的结构体用于数据传送，若所有结构体都要封装成对应的 Python 类，工作量太大也非常容易出错。解决方案是使用 Python 中的 Dict 字典。字典有着更为高级的数据结构，它本质是一个哈希表，同一个字典内键和值的类型允许不同，这个特性使得字典可以非常方便地用来代替 C++的结构体。

原生 API 的函数名开头都是大写字母，为了便于分辨及符合 Python 的 PEP8 编码规则，Python 封装后的函数都以小写字母开头。例如：原生 API 中以 On 开头的回调函数（如 OnRspUserLogin）对应的 Python API 的回调函数直接改为以 on 开头的函数（如 onRspUserLogin）；主动函数（如 ReqUserLogin）对应的封装后 API 中的主动函数改为首字母小写（如 reqUserLogin）。

### 3. 封装后 API 工作流程

（1）主动函数

- 用户在 Python 程序中调用封装 API 的主动函数，并直接传入 Python 变量（PyObject 对象）作为参数。
- 封装 API 将 Python 变量转换成 C++变量。
- 封装 API 调用原生 API 的主动函数，并传入 C++变量作为参数。

（2）回调函数

- 交易柜台通过原生 API 的 C++回调函数推送数据信息，传入参数为 C++变量。
- 封装 API 将 C++变量转换为 Python 变量。
- 封装 API 调用封装后的回调函数向用户的 Python 程序中推送数据，并传入 Python 变量作为参数。

## 3.5.3 CTP API 对接中层引擎原理

当原生 C++的 CTP API 封装成功后，就

- （1）将 API 的回调函数收到的数据推送到程序的中层引擎中，等待处理；
- （2）将 API 的主动函数进行一定的简化封装，便于中层引擎调用。

下面通过案例代码来具体展示 CTP API 对接中层引擎的具体流程。

### 1. 回调函数

通过回调函数收到 API 的数据推送后，创建不同类型的 Event 对象（来自事件驱动引擎模块），在事件对象的数据字典 dict_中保存需要具体推送的数据，然后推送到事件驱动引擎中，由其负责处理。

在回调函数收到的数据中，data 和 error 分别对应的是保存主要数据（如行情）和错误信息的字典，n 是该回调函数对应的请求号（即调用主动函数时的 reqid），last 是一个布尔值，代表是否为该次调用的最后返回信息。

我们主要对 data 字典感兴趣，因此选择在事件中整体推送。

而 error 字典每次收到后应当立即检查是否包含错误信息（因为即使没有发生错误也会推送），若有则自动保存为一个日志事件（通过日志监控控件显示出来）。

服务器连接完成（onFrontConnected）后，检查是否已经填入了用户名等登录信息，若有则自动登录（请参考后面主动函数中的示例）。

登录完成（onRspUserLogin）后，自动订阅_setSubscribed 中之前已经订阅过的合约。

Python 量化交易

收到行情推送（ onRtnDepthMarketData ）后，我们选择创建两种事件：一种是常规行情事件（通常适用于市场行情监控 GUI 等对所有行情推送都关注的组件）；另一种是特定合约行情事件（通常适用于算法等仅关注特定合约行情的组件）。

当调用有返回信息的主动函数时，需要传入本次请求的编号，此时先将 reqid 加 1，再作为参数传入主动函数中。

其示例代码如下：

```
#------------------------------------------------------------
    def onFrontConnected(self):
        """服务器连接"""
        event = Event(type_=EVENT_LOG)
        event.dict_['log'] = u'行情服务器连接成功'
        self.__eventEngine.put(event)

        # 如果用户已经填入了用户名等，则尝试自动登录。
        if self.__userid:
            req = {}
            req['UserID'] = self.__userid
            req['Password'] = self.__password
            req['BrokerID'] = self.__brokerid
            self.__reqid = self.__reqid + 1
            self.reqUserLogin(req, self.__reqid)

......

    #------------------------------------------------------------
    def onRspUserLogin(self, data, error, n, last):
        """登录回报"""
        event = Event(type_=EVENT_LOG)

        if error['ErrorID'] == 0:
            log = u'行情服务器登录成功'
        else:
            log = u'登录回报, 错误代码: ' + unicode(error['ErrorID']) + u', ' + u'错误信息: ' + error['ErrorMsg'].decode('gbk')
```

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

### 2. 主动函数

主动函数仅封装了两个功能：登录（login）和订阅合约（subscribe）。

对于登录函数而言，传入参数包括服务器前置机地址 address、用户名 userid、密码 password，以及经纪商代码 brokerid。函数调用后，先将 userid，password 和 brokerid 保存下来，然后注册服务器地址 registerFront，并初始化连接。连接完成后，onFrontConnected 回调函数会被自动调用，之后发生的操作请参考回调工作流程。

订阅合约方面，CTP API 在期货方只需要传入合约代码，但是证券类的接口需要同时传入合约的代码，以及合约所在的交易所（因为存在两个证券交易所相同代码的情况）。发送订阅请求后，将该订阅请求保存在 setSubscribed 集合中，使得断开重连时可以自动重新订阅。

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

## 3.6 事件引擎

事件引擎是 vn.py 的核心组件，通过对接底层接口和上层应用模块，维持整个交易系统的正常运作。与常见的时间驱动不同，事件引擎的设计基于事件驱动。

### 3.6.1 时间驱动

时间驱动就是让计算机每隔一段时间固定运行一次脚本，脚本自身可以很长、包含非常多的步骤，但是这种程序的运行机制相对比较简单、容易理解。下面提供时间驱动在量化交易领域应用的 2 个例子。

例子 1：每隔 2 分钟，获取交易账户的最新净值、持仓信息、委托状态等。

例子 2：每隔 1 秒钟，检查一次最新收到的股指期货 Tick 数据，更新 K 线和其他技术指标，检查是否满足趋势策略的下单条件，若满足则执行下单。

但是对于速度要求较高的量化交易（如日内 CTA 策略、高频策略等），时间驱动程序却存在一个非常大的缺点：对数据信息在反应操作上的处理延时。在例子 2 中，在每次逻辑脚本运行完等待的那 1 秒钟里，程序对于接收的新数据信息（行情、成交推送等）是不会做出任何反应的，只有在等待时间结束后脚本再次运行时才会进行相关的计算处理，导致交易成本提高。

若是缩小时间驱动程序的等待间隔，提高程序运行的频率。比如在例子 2 中，每隔 100 毫秒检查获取最新股指期货 Tick 数据，尽管可以在一定程度降低滑点费用，却会导致 CPU 计算资源的浪费，大大降低计算机运转性能。

### 3.6.2 事件驱动

所谓事件驱动，简单地说就是你点什么按钮（即产生什么事件），计算机执行什么操作（即调用什么函数）。当然事件不仅限于用户的操作。事件驱动的核心自然是“事件”。从事件角度说，事件驱动程序的基本结构包括事件收集器、事件发送器和事件处理器。事件收集器专门负责收集所有事件，包括来自用户的（如鼠标、键盘事件等）、硬件的（如时钟事件等）和软件的（如操作系统、应用程序本身等）。事件发送器负责将收集器收集到的事件分发到目标对象中。事件处理器做具体的事件响应工作，它往往要到实现阶段才完全确定，因而需要运用虚函数机制。

回到交易系统的设计，事件驱动就是当某个新的事件被推送到程序中时（如 API 推送新的行情、成交等），程序立即调用和这个事件相对应的处理函数进行相关的操作。若没有事件推送过去，该程序是不会被触发的，这样可以完美地解决在上面的例子 2 中 CPU 资源浪费的问题：交易程序对股指期货 Tick 数据进行监听，当没有新的行情过来时，程序保持监听状态不进行任何操作；当收到新的数据时，数据处理函数立即更新 K 线和其他技术指标，并检查是否满足趋势策略的下单条件，据此执行下单。

事件驱动设计的另一个优点就是增强交易系统的扩展性，因为事件驱动引擎可以管理不同事件的事件监听函数并执行所有和事件驱动相关的操作。例如收到 CTP API 推送的最新股指期货 Tick 数据，针对这个数据，系统需要进行如下处理：

- 更新行情监控表中股指期货的行情数据（表格更新）。
- 各个策略均需要运行一次内部算法，检查该数据是否会触发策略执行下单（运算、下单）。
- 风控系统需要检查最新行情价格是否会导致账户的整体风险超限，若超限需要进行报警（运算、报警）。

### 3.6.3 事件引擎工作流程

图 3-24 是由 vn.py 社区贡献的事件引擎工作流程图。

事件驱动流程可以分为两个方面：一方面是用户在主动订阅端通过事件引擎把订阅请求发送到 API 接口；另一方面是回调推送端把最新的 Tick 行情数据由事件引擎推送到具体的交易策略中进行运算。

![](img/111072991fcea5fcc2790f3fbbeb60af_156_0.png)

图 3-24 事件引擎工作流程图

### 1. 主动订阅端

- 用户发起调用 mainEngine.subscribe 函数。
- mainEngine.subscribe 中调用 ctpGateway.subscribe 函数。
- ctpGateway.subscribe 中调用 ctpMdApi.subscribe 函数。
- ctpMdApi.subscribe 中调用 C++封装的 MdApi.subscribeMarketData 函数，将订阅行情的请求最终通过底层 C++ CTP API 发出。

### 2. 回调推送端

- ctaEngine 对象向 eventEngine 中注册 EVENT_TICK 事件类型的处理函数句柄 ctaEngine.processTickEvent。
- C++ CTP API 收到 Tick 推送，自动回调 MdApi.onRtnDepthMarketData 函数推送行情数据字典 data。
- MdApi.onRtnDepthMarketData 中将 data 里的数据读取并转化成 VtTickData 对象，并调用 ctpGateway.onTick 函数。
- ctpGateway.onTick 函数将 VtTickData 对象包装成类型为 EVENT_TICK 的行情事件对象 Event，并调用 eventEngine.put 函数，放入事件引擎的缓冲队列。
- 事件引擎的工作线程，从缓冲队列中读取出最新的行情事件后，根据 EVENT_TICK 事件类型去查找缓存在内部字典中的处理函数列表，并将事件对象作为入参，遍历调用到列表中的处理函数 ctaEngine.processTickEvent。
- ctaEngine.processTickEvent 查看 Tick 的代码 vtSymbol，并调用交易该代码合约的策略对象 strategy.onTick 函数，最终去运行策略中的逻辑。

### 3.6.4 事件引擎结构

事件引擎处于 “vnpy-1.9.0\vnpy\event” 文件夹中的 eventEngine.py 内，打开该文件可以发现有两个版本事件驱动引擎，两个版本的区别仅在于定时器是通过不同的 Python 模块实现的，EventEngine 直接用 QTimer，而 EventEngine2 用线程实现定时器。

整体来说，事件引擎主要由以下 5 部分组成：

- 事件队列(__queue)。
- 事件处理线程(__thread)。
- 事件处理函数字典(__handlers) / 通用事件处理函数列表(__generalHandlers)。
- 定时器(__timer)。
- 引擎开关(__active)。

### 1. 事件队列

事件队列（__queue）是一个队列（Queue）对象。通过事件引擎的 put(event) 方法，用户可向事件队列中传入事件（Event）对象，事件引擎开始运行后会不断尝试从队列中取出事件对象、从事件处理函数字典中寻找对应该事件类型的一个或多个事件处理函数并依次调用之。引擎调用监听对应事件类型的事件处理函数的过程，就是引擎被事件“驱动”的过程。

### 2. 事件处理线程

事件处理线程(__thread)是一个线程(Thread)对象,该线程中运行的是__run()函数，即对事件队列的轮询及事件处理函数的调用都运行在该线程中。将事件处理放到一个单独的线程中是必要的,最主要的原因是当创建事件引擎并调用 start() 函数将之启动的时候，函数可以及时返回而保持事件引擎的连续运行，不会因为事件引擎的运行而阻塞。

### 3. 事件处理函数字典/通用事件处理函数列表

事件处理函数字典（__handlers）是一个字典（dict），用于存储事件类型和事件处理函数的监听关系。事件处理函数字典的 Key 一般是事先定义好的常量，用来表示单一的事件类型，字典的 Value 是列表（list），用来存储处理对应事件类型的处理函数（即一个事件类型可以被多个事件处理函数监听）。用户可以通过 register(type_, handler)和 unregister(type_, handler)这两个函数向事件处理字典中对应事件类型的函数列表中添加或移除事件处理函数，即注册或注销事件处理
