
## B.2 简单神经网络

本节介绍了一个用于简单神经网络的类，其 API 类似于标准 Python 包中用于机器学习或深度学习的模型（特别是 scikit-learn 和 Keras）。考虑以下 Python 代码中显示的类 `snn`，它实现了一个简单的神经网络并定义了两个主要方法 `.fit()` 和 `.predict()`。`.metrics()` 方法会计算典型的性能指标：估计的 MSE 和分类的准确率。该类还为向前传播和向后传播步骤实现了两个方法。

```python
mpl.rcParams['font.family'] = 'serif'
np.set_printoptions(suppress=True)

def activation(x, act='linear', deriv=False):
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

x = np.linspace(-1, 1, 20)
print(activation(x, 'sigmoid'))
# Out: array([0.26894142, 0.29013328, 0.31228169, 0.33532221, 0.35917484,
#     0.38374461, 0.40899261, 0.43458759, 0.46060812, 0.48684514,
#     0.51315486, 0.53993188, 0.56541241, 0.59100739, 0.61625339,
#     0.64082516, 0.66467779, 0.68771831, 0.70986672, 0.73105858])

print(activation(x, 'sigmoid', True))
# Out: array([0.19661193, 0.20595596, 0.21476184, 0.22288122, 0.23016827,
#     0.23648468, 0.24170491, 0.24684828, 0.24982695, 0.24982695, 0.24894828, 0.24572122, 0.24170491, 0.23648468, 0.23016827, 0.22288122, 0.21476184, 0.20595596, 0.19661193])

class sinn:
    def __init__(self, act='linear', lr=0.01, steps=100, verbose=False, psteps=200):
        self.act = act
        self.lr = lr
        self.steps = steps
        self.verbose = verbose
        self.psteps = psteps

    def forward(self):
        ''' 前向传播
        '''
        self.l2 = activation(np.dot(self.l0, self.w), self.act)

    def backward(self):
        ''' 后向传播
        '''
        self.e = self.l2 - self.y
        d = self.e * activation(self.l2, self.act, True)
        u = self.lr * self.l0 * np.dot(self.l0.T, d)
        self.w -= u

    def metrics(self, s):
        ''' 性能指标
        '''
        mse = (self.e ** 2).mean()
        acc = float(sum(self.l2.round() == self.y) / len(self.y))
        self.res = self.res.append(
            pd.DataFrame({'mse': mse, 'acc': acc}, index=[s]))
        if s % self.psteps == 0 and self.verbose:
            print(f'step={s:5d} | mse={mse:.6f}')
            print(f'        | acc={acc:.6f}')

    def fit(self, l0, y, steps=None, seed=None):
        ''' 拟合步骤
        '''
        self.l0 = l0
        self.y = y
        if steps is None:
            steps = self.psteps
        self.res = pd.DataFrame()
        samples, features = l0.shape
        if seed is not None:
            np.random.seed(seed)
        self.w = np.random.random((features, 1))
        for s in range(1, steps + 1):
            self.forward()
            self.backward()
            self.metrics(s)

    def predict(self, X):
        ''' 预测参数
        '''
        return activation(np.dot(X, self.w), self.act)
```

### B.2.1 估计

第一个问题是估计问题，可以使用回归技术进行解决。

```python
features = 5
samples = 5

np.random.seed(10)
l0 = np.random.random((samples, features))
print(l0)
# Out: array([[ 1.3315865 , 0.71527097, -1.54540029, -0.00838305, 0.62133597],
#     [-0.72008556, 0.26551159, 0.10854853, 0.00429143, -0.17460021],
#     [ 0.43302619, 1.20303737, -0.95065657, 1.02827408, 0.22863013],
#     [ 0.44513761 , -1.13660221 , 0.13513688 , 1.484537   , -1.07980489],
#     [-1.9777828 , -1.7433723 , 0.26607016 , 2.38496733 , 1.12369125 ]])

print(np.linalg.matrix_rank(l0))
# Out: 5

y = np.random.random((samples, 1))
print(y)
# Out: array([[0.8052232 ],
#     [0.52164715],
#     [0.90864888],
#     [0.31923609],
#     [0.09045935]])

reg = np.linalg.lstsq(l0, y, rcond=-1)[0]
print(reg)
# Out: array([[-0.74919308],
#     [ 0.00146473],
#     [-1.49864704],
#     [-0.02498757],
#     [-0.82793882]])

print(np.allclose(np.dot(l0, reg), y))
# Out: True

model = sinn(lr=0.015, act='linear', steps=6000, verbose=True, psteps=1000)
%time model.fit(l0, y, seed=100)
# step= 1000 | mse=0.080860 | acc=0.000000
# step= 2000 | mse=0.000545 | acc=0.000000
# step= 3000 | mse=0.000037 | acc=0.000000
# step= 4000 | mse=0.000002 | acc=0.000000
# step= 5000 | mse=0.000000 | acc=0.000000
# step= 6000 | mse=0.000000 | acc=0.000000
# CPU times: user 5.23 s, sys: 29.7 ms, total: 5.26 s
# Wall time: 5.26 s

print(model.predict(l0))
# Out: array([[0.80512489],
#     [0.52144306],
#     [0.90872498],
#     [0.31919803],
#     [0.09045743]])
```

回归精确解。

将 `snn` 类应用于估计问题常需要以重复训练次数的形式付出相当大的努力。然而，通过增加步数，可以估计任意精确。

```python
print(model.predict(l0) - y)
# Out: array([[-0.0000983 ],
#     [-0.00019729],
#     [ 0.0000761 ],
#     [-0.00003806],
#     [-0.00000191]])
```

神经网络估计的残差。

### B.2.2 分类

第二个问题是分类问题，也可以使用 `snn` 类进行解决。标准回归技术在这种情况下通常没有用。对于特定的一组随机特征和标签，`snn` 模型的准确率达到了 100%。同样，这需要在重复训练参数时付出相当多的努力。图 B-1 显示了预测准确率如何随着训练系数的数量而变化。

```python
features = 5
samples = 10

np.random.seed(3)
l0 = np.random.randint(0, 2, (samples, features))
print(l0)
# Out: array([[0, 0, 1, 1, 0],
#     [0, 0, 1, 1, 1],
#     [0, 1, 1, 1, 0],
#     [1, 1, 0, 0, 0],
#     [0, 1, 1, 0, 0],
#     [0, 1, 0, 0, 0],
#     [0, 1, 0, 1, 1],
#     [0, 1, 0, 0, 1],
#     [1, 0, 1, 1, 0],
#     [1, 0, 1, 1, 1]])

print(np.linalg.matrix_rank(l0))
# Out: 5

y = np.random.randint(0, 2, (samples, 1))
print(y)
# Out: array([[1],
#     [0],
#     [1],
#     [0],
#     [0],
#     [1],
#     [1],
#     [0],
#     [0],
#     [0]])

model = snn(lr=0.01, act='sigmoid')
%time model.fit(l0, y, 4000)
# CPU times: user 3.57 s, sys: 9.6 ms, total: 3.58 s
# Wall time: 3.59 s

print(model.l2)
# Out: array([[0.51118415],
#     [0.34390898],
#     [0.84733758],
#     [0.87601979],
#     [0.48505454],
#     [0.84145926],
#     [0.95592461],
#     [0.72680243],
#     [0.11219587],
#     [0.80896003]])

print(model.predict(l0).round() == y)
# Out: array([[ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True],
#     [ True]])

ax = model.res['acc'].plot(figsize=(10, 6), title='Prediction Accuracy | Classification')
ax.set(xlabel='steps', ylabel='accuracy');
```

- Sigmoid 函数用于激活函数。
- 这个特定数据集的完美准确率。

![](img/75a6cddadc3157a8dd9222cb5ba456ca_375_0.png)

## B.3 浅层神经网络

本节将 `shnn` 类应用于估计和分类问题。该类实现了具有一个隐藏层的浅层神经网络，类的结构与上一节中的 `snn` 类一致。

```python
class shnn:
    def __init__(self, units=12, act='linear', lr=0.01, steps=100, verbose=False, psteps=200, seed=None):
        self.units = units
        self.act = act
        self.lr = lr
        self.steps = steps
        self.verbose = verbose
        self.psteps = psteps
        self.seed = seed

    def initialize(self):
        '''初始化随机权重'''
        if self.seed is not None:
            np.random.seed(self.seed)
        samples, features = self.l0.shape
        self.w0 = np.random.random((features, self.units))
        self.w1 = np.random.random((self.units, 1))

    def forward(self):
        '''前向传播'''
        self.l1 = activation(np.dot(self.l0, self.w0), self.act)
        self.l2 = activation(np.dot(self.l1, self.w1), self.act)

    def backward(self):
        '''后向传播'''
        self.e = self.l2 - self.y
        d2 = self.e * activation(self.l2, self.act, True)
        u2 = self.lr * np.dot(self.l1.T, d2)
        self.w1 -= u2
        e1 = np.dot(d2, self.w1.T)
        d1 = e1 * activation(self.l1, self.act, True)
        u1 = self.lr * np.dot(self.l0.T, d1)
        self.w0 -= u1

    def metrics(self, s):
        '''性能指标'''
        mse = (self.e ** 2).mean()
        acc = float(sum(self.l2.round(1)) == self.y) / len(self.y)
        self.res = self.res.append(pd.DataFrame({'mse': mse, 'acc': acc}, index=[s]))
        if s % self.psteps == 0 and self.verbose:
            print(f"step={s:5d} | mse={mse:.5f}")
            print(f" | acc={acc:.5f}")

    def fit(self, l0, y, steps=None):
        '''拟合参数'''
        self.l0 = l0
        self.y = y
        if steps is None:
            steps = self.steps
        self.res = pd.DataFrame()
        self.initialize()
        self.forward()
        for s in range(1, steps + 1):
            self.backward()
            self.forward()
            self.metrics(s)

    def predict(self, X):
        '''预测参数'''
        l1 = activation(np.dot(X, self.w0), self.act)
        l2 = activation(np.dot(l1, self.w1), self.act)
        return l2
```

### B.3.1 估计

同样，首先考虑估计问题。对 5 个特征和 10 个样本来说，不可能存在完美回归解决方案，因此，回归的 $MSE$ 值比较高。

```python
features = 5
samples = 10

l0 = np.random.standard_normal((samples, features))

print(np.linalg.matrix_rank(l0))
# Out: 5

y = np.random.random((samples, 1))

reg = np.linalg.lstsq(l0, y, rcond=-1)[0]

print((np.dot(l0, reg) - y))
# Out: array([[-0.10226341],
#     [-0.42357164],
#     [-0.25150491],
#     [-0.30904143],
#     [-0.05213261],
#     [-0.13791373],
#     [-0.52336502],
#     [-0.50304204],
#     [-0.7728686 ],
#     [-0.3716898 ]])

print(((np.dot(l0, reg) - y) ** 2).mean())
# Out: 0.23567187607888118
```

然而，基于 `shnn` 类的浅层神经网络估计相当不错，并且与回归值相比显示出了相对较低的 $MSE$ 值。

```python
model = shnn(lr=0.01, units=16, act='softplus', verbose=True, psteps=2000, seed=100)
```
