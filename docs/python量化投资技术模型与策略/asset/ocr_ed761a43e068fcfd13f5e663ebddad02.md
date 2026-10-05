输出结果如下：

```
{0: '玻璃行业',1: '船舶制造',2: '传媒娱乐',3: '电力行业'}
```

输入的数据准备好了。现在就可以调用整合函数进行整理了，代码如下：

```
# 将数据整合为alphalens所需要的格式
factor_data =
alphalens.utils.get_clean_factor_and_forward_returns(predictive_factor,

pricing,

quantiles=5,

bins=None,

groupby=code_sec,
groupby_labels=sec_names)
factor_data.head()
```

输出结果如图14-2所示。