# 从零开始学 Python 大数据与量化交易

```
factors = calc_factors(securities, [myalpha()], start_date='2018-04-01', end_date='2018-04-13')
factors['myalpha1'].head()
```

单击 运行 按钮，运行代码，显示结果如图 15.17 所示。

![](bbox=[0.17, 0.19, 0.875, 0.395])

# 图 15.17 因子在研究中的使用

# 15.5 基本面因子应用实例

在浏览器的地址栏中输入 https://www.joinquant.com，然后按 Enter 键，就进入 JoinQuant（聚宽）量化交易平台的首页页面。

在菜单栏中选择 “我的策略” → “单因子分析” 命令，进入 “我的因子” 页面。再单击 “新建因子” 按钮，就 可以新建一个因子，然后编写 Python 代码，具体如下：

```
from jqfactor import Factor
class mygross(Factor):
    # 设置因子名称
    name = 'mygross 1'
    # 设置获取数据的时间窗口长度
    max_window = 1
    # 设置依赖的数据
    dependencies = ['total_operating_revenue','total_operating_cost','total_assets']
    # 计算因子的函数，需要返回一个 pandas.Series, index 是股票代码，value 是因子值
    def calc(self, data):
        # 获取单季度的营业总收入数据，index 是日期，column 是股票代码，value 是营业总收入
        total_operating_revenue = data['total_operating_revenue']
        # 获取单季度的营业总成本数据
        total_operating_cost = data['total_operating_cost']
```