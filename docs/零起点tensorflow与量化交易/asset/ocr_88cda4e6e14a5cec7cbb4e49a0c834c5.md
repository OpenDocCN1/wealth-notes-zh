```
```
3.310   100.000   2
    3068  3.310  3.320  3.300  3.320  940023.000  3.310  3.310  3.290
3.300 ...      1     1     3     1     0  2.000  3.310
3.300      99.698  2
    3069  3.290  3.320  3.280  3.320 1136877.000  3.300  3.310  3.310  3.300
3.290 ...      1     1     3     1     0  2.000  3.300
3.290      99.697  2
[5 rows x 27 columns]
x_train.shape, (120950, 22)
type(x_train), <class 'numpy.ndarray'>
```
```

第 2.2 组代码如下：

```
```
#2.2
print('\n#2.2,转换数据格式 shape')
rxn,txn=x_train.shape[0],x_test.shape[0]
x_train,x_test = 
x_train.reshape(rxn,num_in,-1),x_test.reshape(txn,num_in,-1)
print('\nx_train.shape,',x_train.shape)
print('\ntype(x_train),',type(x_train))
```
```

转换数据格式 shape。因为 LSTM 模型的输入数据格式是三维数组，所以需要调用 NumPy 的 reshape 函数修改数据格式 shape。

其对应的输出信息如下：

```
```
#2.2,转换数据格式 shape
x_train.shape, (120950, 22, 1)
type(x_train), <class 'numpy.ndarray'>
```
```

和第 2.1 组代码的最后几行输出信息对比，可以发现 x_train 训练数据集的格式，已经由二维数组变为三维数组。

第 3 组代码如下：

```
```
#3
print('\n#3,建立神经网络模型')
mx=zks.lstm020typ(num_in,num_out)
#
mx.summary()
plot_model(mx, to_file='tmp/lstm020.png')
```
```