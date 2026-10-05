```
# 计算快速均线
fast_ma = am.sma(self.fast_window, array=True)
self.fast_ma0 = fast_ma[-1]      # T时刻数值
self.fast_ma1 = fast_ma[-2]      # T-1时刻数值

# 计算慢速均线
slow_ma = am.sma(self.slow_window, array=True)
self.slow_ma0 = slow_ma[-1]
self.slow_ma1 = slow_ma[-2]
```

注意，这里我们传入了可选参数array=True，因此返回的fast_ma为最新移动平均线的数组，其中最新一个周期（T时刻）的移动均线ma数值可以通过-1下标来获取，上一个周期（T-1时刻）的ma数值可以通过-2下标来获取。

有了快慢两根均线在T时刻和T-1时刻的数值之后，我们就可以进行双均线策略的核心逻辑判断了，即判断是否发生了均线金叉或者死叉，代码如下：

```
# 判断是否金叉
cross_over = (self.fast_ma0 > self.slow_ma0 and
              self.fast_ma1 < self.slow_ma1)

# 判断是否死叉
cross_below = (self.fast_ma0 < self.slow_ma0 and
               self.fast_ma1 > self.slow_ma1)
```

所谓的均线金叉，是指T-1时刻的快速均线fast_ma1低于慢速均线slow_ma1，而T时刻时快速均线fast_ma0大于或等于慢速均线