```
margin=0.1
ax.text(A[0]+margin, A[1]+margin, r"A",fontsize=20)
ax.text(C[0]+margin, C[1]+margin, r"C",fontsize=20)
ax.text(P[0]+margin, P[1]+margin, r"P",fontsize=20)
ax.text(0+margin,0+margin,r"O",fontsize=20)
ax.text(0+margin,4+margin, r"y",fontsize=20)
ax.text(4+margin,0+margin, r"x",fontsize=20)
plt.xticks(np.arange(-2,3))
plt.yticks(np.arange(-2,3))
ax.axis('equal')
plt.show()
```

最终结果如图 9-5 所示。

![](bbox=[0.295, 0.355, 0.722, 0.54])

在【程序 9-1】中，详细演示了最小二乘法的计算，但是在实际应用中并不需要每次使用最小二乘法进行计算时都需要列出详细的方法，Python 有专门的工具包为读者提供计算最小二乘法的方法。

代码段如下：

```
import statsmodels.api as sm
import numpy as np

def 一元线性回归():
    x1 = np.asarray([1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
    y = 10 + 4 * x1

x_ = sm.add_constant(x1)
    model = sm.OLS(y, x_)
results = model.fit()

    print(results.params)
```