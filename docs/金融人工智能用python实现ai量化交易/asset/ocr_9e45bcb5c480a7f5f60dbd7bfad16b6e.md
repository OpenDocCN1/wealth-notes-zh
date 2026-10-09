```
def forward(self):
    '''  前向传播
    '''

self.l2 = activation(np.dot(self.l0, self.l0), self.act)
def backward(self):
    '''  后向传播
    '''
self.e = self.l2 - self.y
d = self.e * activation(self.l2, self.act, True)
u = self.l0 * np.dot(self.l0.T, d)
self.l0 -= u
def metrics(self, s):
    '''  性能指标
    '''

mse = (self.e ** 2).mean()
acc = float(sum(self.l2.round() == self.y) / len(self.y))
self.res = self.res.append(
        pd.DataFrame({'mse': mse, 'acc': acc}, index=[s]))
if s % self.psteps == 0 and self.verbose:
    print(f'step={s:5d} | mse={mse:.6f}')
print(f'        | acc={acc:.6f}')
def fit(self, l0, y, steps=None, seed=None):
    '''  拟台步骤
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
    '''  预测参数
    '''

return activation(np.dot(X, self.w), self.act)
```

# B.2.1 估计

第一个问题是估计问题，可以使用回归技术进行解决。

```
In [7]: features = 5
samples = 5

In [8]: np.random.seed(10)
10
```

```
Out[8]: array([[ 1.3315865 , 0.71527097, -1.54540029, -0.00838305, 0.62133597],
        [-0.72008556, 0.26551159, 0.10854853, 0.00429143, -0.17460021],
        [ 0.43302619, 1.20303737, -0.95065657, 1.02827408, 0.22863013]],
```