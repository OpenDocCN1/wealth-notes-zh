![](bbox=[0.106, 0.1, 0.85, 0.4])

图 13.17 因子在研究中的使用

# 13.5 基本面因子运用实例

在浏览器的地址栏中输入 “https://www.joinquant.com”，然后按 “Enter” 键，即可进入 JoinQuant 聚宽量化交易平台的首页。登录成功后，单击导航栏中的 “策略研究” → “单因子分析” 命令，进入 “我的因子” 页面。再单击 “新建因子” 按钮，即可新建一个因子，然后编写 Python 代码，具体如下：

```
from jqfactor import Factor
class mygross(Factor):
    # 设置因子名称
    name = ' mygross 1'
    # 设置获取数据的时间窗口长度
    max_window = 1
    # 设置依赖的数据
    dependencies = ['total_operating_revenue','total_operating_cost','total_assets']
    # 计算因子的函数，需要返回一个 pandas.Series，index 是股票代码，value 是因子值
    def calc(self, data):
        # 获取单季度的营业总收入数据;index 是日期, column 是股票代码, value 是营业总收入
        total_operating_revenue = data['total_operating_revenue']
        # 获取单季度的营业总成本数据
        total_operating_cost = data['total_operating_cost']
        # 获取总资产
        total_assets = data['total_assets']
```