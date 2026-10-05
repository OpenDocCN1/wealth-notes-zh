```
setting):
    """"
    super().__init__(cta_engine, strategy_name, vt_symbol,
setting)

    # K线合成器：用于从Tick合成分钟K线
    self.bg = BarGenerator(self.on_bar)

    # 时间序列容器：用于计算技术指标
    self.am = ArrayManager()

def on_init(self):
    """
    当策略被初始化时调用该函数
    """
    # 输出日志，下同
    self.write_log("策略初始化")

    # 加载10天的历史数据用于初始化回放
    self.load_bar(10)

def on_start(self):
    """
    当策略被启动时调用该函数
    """
    self.write_log("策略启动")

    # 通知图形界面更新（策略的最新状态）
    # 若不调用该函数则界面不会发生变化
    self.put_event()

def on_stop(self):
    """
    当策略被停止时调用该函数
    """
    self.write_log("策略停止")

    self.put_event()

def on_tick(self, tick: TickData):
    """
    通过该函数收到Tick推送
    """
    self.bg.update_tick(tick)

def on_bar(self, bar: BarData):
    """
    通过该函数收到新的1分钟K线推送
```