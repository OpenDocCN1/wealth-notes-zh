```
mpl.rcParams['font.family'] = 'serif'
np.set_printoptions(suppress=True)
```

```
In [2]: def activation(x, act='linear', deriv=False):
    if act == 'sigmoid':
        if deriv:
            out = activation(x, 'sigmoid', False)
            return out * (1 - out)
        return 1 / (1 + np.exp(-x))
    elif act == 'relu':
        if deriv:
            return np.where(x > 0, 1, 0)
        return np.maximum(x, 0)
    elif act == 'softplus':
        if deriv:
            return activation(x, act='sigmoid')
        return np.log(1 + np.exp(x))
    elif act == 'linear':
        if deriv:
            return 1
        return x
    else:
        raise ValueError('Activation function not known.')
```

```
In [3]: x = np.linspace(-1, 1, 20)
```

```
In [4]: activation(x, 'sigmoid')
Out[4]: array([0.26894142, 0.29013328, 0.31228169, 0.33532221, 0.35917484,
    0.38374461, 0.40899261, 0.43458759, 0.46060812, 0.48684514,
    0.51315486, 0.53993188, 0.56541241, 0.59100739, 0.61625339,
    0.64082516, 0.66467779, 0.68771831, 0.70986672, 0.73105858])
```

```
In [5]: activation(x, 'sigmoid', True)
Out[5]: array([0.19661193, 0.20595596, 0.21476184, 0.22288122, 0.23016827,
    0.23648468, 0.24170491, 0.24684828, 0.24982695, 0.24982695, 0.24894828, 0.24572122, 0.24170491, 0.23648468, 0.23016827, 0.22288122, 0.21476184, 0.20595596, 0.19661193])
```

# ## B.2 简单神经网络

本节介绍了一个用于简单神经网络的类，其 API 类似于标准 Python 包中用于机器学习或深度学习的模型（特别是 scikit-learn 和 Keras）。考虑以下 Python 代码中显示的类 sinn，它实现了一个简单的神经网络并定义了两个主要方法 .fit() 和 .predict()。.metrics() 方法会计算典型的性能指标：估计的 MSE 和分类的准确率。该类还为向前传播和向后传播步骤实现了两个方法。

```
In [6]: class sinn:
    def _init_(self, act='linear', lr=0.01, steps=100, verbose=False, psteps=200):
        self.act = act
        self.lr = lr
        self.steps = steps
        self.verbose = verbose
        self.psteps = psteps
```