基于训练好的RNN，以下Python代码生成了样本内预测和样本外预测。

```
In [18]: x = np.array([[21, 22, 23]]).reshape((1, lags, 1))
          y = model.predict(x, verbose=False)
          int(round(y[0, 0]))
Out[18]: 24
```

```
In [19]: x = np.array([[87, 88, 89]]).reshape((1, lags, 1))
          y = model.predict(x, verbose=False)
          int(round(y[0, 0]))
Out[19]: 90
```

```
In [20]: x = np.array([[187, 188, 189]]).reshape((1, lags, 1))
          y = model.predict(x, verbose=False)
          int(round(y[0, 0]))
Out[20]: 190
```

```
In [21]: x = np.array([[1187, 1188, 1189]]).reshape((1, lags, 1))
          y = model.predict(x, verbose=False)
          int(round(y[0, 0]))
Out[21]: 1194
```

+   样本内预测。

样本外预测。

远离样本的预测。

即使对于远离样本的预测，在这种简单的情形下，结果通常也很好。然而，同样的问题通过OLS回归也可以完美解决。因此，考虑到RNN的性能，针对此类问题训练RN所涉及的工作量非常大。

# ## 8.2 第二个示例

第一个示例说明了针对简单问题的RNN训练：该问题不仅可以通过OLS回归轻松解决，也可以通过人工检查数据轻松解决。第二个示例更具挑战性，输入数据通过二次项和三角项进行转换，并添加了白噪声。图8-2显示了区间[-2π，2π]的序列值。

```
In [22]: def transform(x):
           y = 0.85 * x ** 2 + 0.2 * x + np.sin(x) + 5
           y += np.random.standard_normal(len(x)) * 0.2
           return y
```

```
In [23]: x = np.linspace(-2 * np.pi, 2 * np.pi, 500)
          a = transform(x)
```

```
In [24]: plt.figure(figsize=(10, 6))
          plt.plot(x, a)
```

+   确定性变换。

随机变换。