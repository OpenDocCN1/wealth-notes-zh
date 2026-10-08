续表
| 序号 | 销售额/千美元 $y_i$ | 学生数/千人 $x_i$ | $x_iy_i$ | $x_i^2$ |
| :---: | :---: | :---: | :---: | :---: |
| 8 | 169 | 20 | 3380 | 400 |
| 9 | 149 | 22 | 3278 | 484 |
| 10 | 202 | 26 | 5252 | 676 |
| 合计 | 1300 | 140 | 21 040 | 2528 |

根据表 3-1 中的数据可得

$$ b = \frac{\sum_{i=1}^{n} x_i y_i - \left( \sum_{i=1}^{n} x_i \sum_{i=1}^{n} y_i \right) / n}{\sum_{i=1}^{n} x_i^2 - \left( \sum_{i=1}^{n} x_i \right)^2 / n} = \frac{21\ 040 - (140 \times 1300) / 10}{2528 - 140^2 / 10} = 2840 / 568 = 5 $$

$$ a = \overline{y} - b\overline{x} = 1300 / 10 - 5 \times 140 / 10 = 60 $$

则得到一元线性回归模型为

$$ \hat{y} = 60 + 5x \hspace{2cm}(3-2) $$

例如，当学生人数为 30 时，根据此模型，销售额是 210(单位是千美元)。

使用 NumPy 编制 Python 代码如下：

```
```
import numpy as np
def fitSLR(x, y):
    n = len(x)
    dinominator = 0
    numerator = 0
    for i in range(0, n):
        numerator += (x[i] - np.mean(x)) * (y[i] - np.mean(y))
        dinominator += (x[i] - np.mean(x)) * * 2
    b1 = numerator/float(dinominator)
    b0 = np.mean(y)/float(np.mean(x))
    return b0, b1
def predict(x, b0, b1):
    return b0 + x * b1
x = [1, 3, 2, 1, 3]
y = [14, 24, 18, 17, 27]
b0, b1 = fitSLR(x, y)
print "intercept:", b0, " slope:", b1
x_test = 6
y_test = predict(6, b0, b1)
print "y_test:", y_test
```
```

执行上述代码，结果如下：

```
```
intercept: 10.0  slope: 5.0
y_test: 40.0
```
```

# ## 练习题

对本章中例题数据，使用 Python 重新操作一遍。