```
theta1 = 0
theta2 = 0

while True:
    cnt += 1

    for i in range(m):
        diff[0] = (theta0 * x[i][0] + theta1 * x[i][1] + theta2 * x[i][2]) - y[i]
        theta0 -= alpha * diff[0] * x[i][0]
        theta1 -= alpha * diff[0] * x[i][1]
        theta2 -= alpha * diff[0] * x[i][2]

    error1 = 0
    for lp in range(len(x)):
        error1 += (y[lp] - (theta0 + theta1 * x[lp][1] + theta2 * x[lp][2])) ** 2 / 2
    if abs(error1 - error0) < epsilon:
        break
    else:
        error0 = error1

print('theta0 : %f, theta1 : %f, theta2 : %f, error1 : %f' % (theta0, theta1, theta2, error1))
print('Done: theta0 : %f, theta1 : %f, theta2 : %f' % (theta0, theta1, theta2))
print('迭代次数: %d' % cnt)
```

最终结果打印如下：

```
theta0 : 0.100684, theta1 : 1.564907, theta2 : 1.920652, error1 : 0.569459
Done: theta0 : 0.100684, theta1 : 1.564907, theta2 : 1.920652
迭代次数: 2118
```

从结果上看，这里需要迭代 2118 次即可获得最优解。

而使用 TensorFlow 计算一元线性回归和多元线性回归的方法如下：

```
#使用 TensorFlow 进行一元线性回归
def linear_regression(real):

    xs = []
    for _ in range(len(real)):
        xs.append(_)
    xs = np.reshape(xs, (-1,1))
    ys = real
    ys = np.reshape(ys, (-1, 1))
```