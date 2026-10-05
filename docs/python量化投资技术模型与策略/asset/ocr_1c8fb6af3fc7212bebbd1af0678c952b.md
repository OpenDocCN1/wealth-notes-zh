# 14.13.3 信息系数分析

信息系数（Information Coefficient Analysis）是衡量因子有效性的另外一个角度。信息系数与相关系数比较相似，都是用于衡量两个变量的线性关系。信息系数在0到1之间，0表示因子没有任何预测作用，1表示因子有完全的预测作用。信息系数越大，表明因子的预测效果越好。

可以用以下函数计算信息比率：

```
ic =
 alphalens.performance.factor_information_coefficient(factor_data)
 ic.head()
```

计算结果如图14-7所示。