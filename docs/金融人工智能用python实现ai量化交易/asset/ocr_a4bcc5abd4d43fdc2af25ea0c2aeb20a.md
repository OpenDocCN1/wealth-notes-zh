我们可以利用对于所逼近函数具体形式的了解，在回归中添如更多的基函数来达到“完美逼近”，也就是说，通过 OLS 回归可以分别恢复原始函数中二次项和三次项系数的精确值。

# ### 1.2.2 神经网络估计

然而，并不是所有关系都是简单的线性关系或者高阶线性关系，这时就需要借助神经网络（neural network, NN）等方法来进行建模。无须赘述，神经网络可以在不需要知道函数关系具体形式的条件下近似各种函数关系。

+   1. scikit-learn

以下 Python 代码使用了 scikit-learn 库中的 MLPRegressor 类，该类可用 DNN 进行回归估计。DNN 有时也被称为多层感知器（multi-layer perceptron, MLP）。⁴ 如图 1-5 中的 MSE 所示，结果并不完美，但是对于一个配置简单的模型来说，效果已经非常不错了。

```
```python
In [29]: from sklearn.neural_network import MLPRegressor
In [30]: model = MLPRegressor(hidden_layer_sizes=3 * [256],
                            learning_rate_init=0.03,
                            max_iter=5000) ①
In [31]: model.fit(x.reshape(-1, 1), y) ②
Out[31]: MLPRegressor(hidden_layer_sizes=[256, 256, 256], learning_rate_init=0.03,
                      max_iter=5000)
In [32]: y_ = model.predict(x.reshape(-1, 1)) ③
In [33]: MSE = ((y - y_) ** 2).mean()
Out[33]: 0.82166235574435866
In [34]: plt.figure(figsize=(10, 6))
          plt.plot(x, y, 'ro', label='sample data')
          plt.plot(x, y_, lw=3.0, label='dnn estimation')
          plt.legend();
```
```

+   ① 实例化 MLPRegressor 对象。
② 拟合或学习步骤。
③ 预测步骤。

> 注 4：有关详细信息，请参阅 sklearn.neural_network.MLPRegressor。如需了解更多背景信息，请参阅 Goodfellow 等（2016）的第 6 章。