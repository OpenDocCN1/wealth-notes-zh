```
In [50]: res = pd.DataFrame(model.history.history)
```

```
In [51]: res.tail()
Out[51]:  val_loss     loss
1375 0.000854 0.000544
1376 0.000685 0.000473
1377 0.001326 0.000942
1378 0.001626 0.000867
1379 0.000710 0.000500
```

```
In [52]: res.iloc[35:25]. plot(figsize=(10, 6))
plt.ylabel('MSE')
plt.xlabel('epochs')
```

![](bbox=[0.14, 0.29, 0.86, 0.59])

图 5-10: DNN 模型在训练数据集和验证数据集上的 MSE 值

在 OLS 回归的情况下，可能会为阶数参数选择一个高但不太高的值，比如 deg=9。神经网
络模型的参数化会在训练结束时自动给出最佳模型配置。图 5-11 比较了两个模型之间的预
测以及与测试数据集的预测。鉴于样本数据的性质，神经网络的测试数据集性能更好一点
儿并不令人惊讶。

```
In [53]: p_ols = np.polyval(reg[5], f_te)
p_dnn = model.predict(f_te).flatten()
```

```
In [54]: MSE(l_te, p_ols)
Out[54]: 0.0038960346771028356
```

```
In [55]: MSE(l_te, p_dnn)
Out[55]: 0.000705705678438721
```

```
In [56]: plt.figure(figsize=(10, 6))
plt.plot(f_te, l_te, 'ro', label='test data')
```