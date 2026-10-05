# Python 机器学习与量化投资

```
y=df_train['y'].values

xtest0=df_test['close'].values
```

此处按时间切分反向训练数据和测试数据，并准备机器学习需要的变量，这些和案例 4-1 类似。

第 5 组代码如下：

```
#5
print('#5, 模型实例')
rx = linear_model.LogisticRegression()
```

调用 sklearn 的 LogisticRegression（逻辑回归）函数，生成相关的 scholarship，并保存到变量 mx 中。

第 6 组代码如下：

```
#6
print('#6, 数据处理')
ss = StandardScaler()
x = ss.fit_transform(x0)
xtest = ss.transform(xtest0)
```

因为逻辑回归模型对数据有严格要求，使用前要进行数据处理。

如果省略以上代码，模型运行会出错。

第 7 组代码如下：

```
#7
print('#7, 训练模型')
mx.fit(x,y)
```

调用模型内建的 fit 函数，根据输入数据进行训练。

第 8 组代码如下：

```
#8
print('#8, predict 模型预测结果')
df_test['y2'] = mx.predict(xtest) #cross_val_predict(mx,x, y, cv=20)

predicted = cross_val_predict(mx,x, y, cv=10)
df['y2'] = predicted
```