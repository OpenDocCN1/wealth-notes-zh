trader\app\ctaStrategy" 文件夹的 ctaTemplate.py 文件内。CTA 策略模块分为 4 部分，分别是定义成员变量、构键函数、回调函数和主动函数。

# 4.4.1 定义成员变量

分别定义如下成员变量：

+   - 策略类的名称和作者。
- MongoDB 数据库名称。
- 策略的基本参数。
- 参数名称列表。
- 变量名称列表。
- 同步列表。

需要注意的一个问题是，用户定义的参数和变量列表属于类的属性，在实例初始化的时候会直接指向该类的同名变量，导致多个实例共享同一个数据容器，策略执行出错。解决方法是将所有的可变变量的定义（如列表、字典等）放在初始化函数__init__中。

# 例子 1：错误用法

```
class TestStrategy(CtaTemplate):
    """CTA 策略模板"""
    ...
    # 这里定义的变量是类成员，方便引擎在创建策略对象前了解一些信息
    barList = []  # 若基于 TestStrategy 类创建多个策略对象，它们的 barList 都会指向同一个列表，导致出错
    lastPrice = 0  # 数字(int、float)在 Python 中属于不可变对象，因此每个策略的 lastPrice 互不影响
    #----------------------------------------------------------------
    def __init__(self, ctaEngine, setting):
        super(TestStrategy, self).__init__(ctaEngine, setting)
```