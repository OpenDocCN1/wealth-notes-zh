# 7.6.2 全局对象 g

全局对象 g，用来存储用户的各类可被 pickle.dumps 函数序列化的全局数据。

在模拟盘过程中，如果进程中断，可以使用 pickle.dumps 序列化所有全局对象 g 下面的变量内容，并将其保存到磁盘中，再启动的时候模拟盘的数据将不会有任何影响。如果没有用全局对象 g 声明，模拟盘重启后，将出现变量数据丢失的问题。

如果不想全局对象 g 中的某个变量被序列化，可以让该变量以 “__” 开头，这样，该变量在序列化时就会被忽略。

全局对象全局对象 g 的实例代码如下：

```
def initialize(context):
    g.security = "000001.XSHE"
    g.count = 1
    g.flag = 0
def process_initialize(context):
    # 保存不能被序列化的对象，每次进程重启都初始化
    g.__q = query(valuation)
def handle_data(context, data):
    log.info(g.security)
    log.info(g.count)
    log.info(g.flag)
```

# 7.6.3 Trade 对象

Trade 对象用于记录订单的一次交易。但需要注意的是，一个订单可能分多次交易。Trade 对象的常用属性如下。

+   - time: 交易时间。
- amount: 交易数量。
- price: 交易价格。
- trade_id: 交易记录 ID。
- order_id: 对应的订单 ID。

# 7.6.4 tick 对象

tick 中的信息是在 tick 事件发生时盘面的一个快照，tick 对象的常用属性如下。

code: 标的的代码。