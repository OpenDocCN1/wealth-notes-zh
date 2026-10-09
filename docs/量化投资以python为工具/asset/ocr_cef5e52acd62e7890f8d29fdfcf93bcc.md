![](bbox=[0.1, 0.09, 0.9, 0.4])

图 24.4 标准化残差的 ACF

![](bbox=[0.1, 0.44, 0.9, 0.75])

图 24.5 调整后的自相关系数图

```
```python
In [32]: LjungBox[1][-1]
Out[32]: 0.015721991173160064

#增加Ljung-Box 检验的滞后阶数
In [33]: LjungBox=statttools.q_stat(statttools.acf(stdresid)[1:20],len(stdresid))

In [34]: LjungBox[1][-1]
Out[34]: 0.0034127263644974696
```
```