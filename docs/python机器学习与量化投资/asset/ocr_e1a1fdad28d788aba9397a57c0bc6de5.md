# Python 机器学习与量化投资

第 5 段代码如下：

```
#5
print('4, 没必要了')
rx = Birch(n_cluster=3)
```

调用 BIRCH 分类函数生成模型，并保存到变量 mx 中。

第 6 段代码如下：

```
#6
print('6, 拟合训练模型')
rx.fit(x, y)
```

输入训练数据集，对模型进行训练，通过输入数据不断调优模型内部参数。

第 7 段代码如下：

```
#7
print('7, predict 模型预测')
df_test['close_pred'] = mx.fit_predict(xtest)
```

模型训练完成后，调用模型内部的 fit_predict 函数生成，生成验证数据。

第 8 段代码如下：

```
#8
print('\n^8, 预测精度验证结果')
dacc, df, xlst = zai.ai_acc_xed2ext(df_test('close_next'), df_test['close_pred'], , True)
```

验证 BIRCH 分类函数的预测结果，对应的输出信息如下：

```
#8, 0.1. 校验验证结果
ky0-1; n_d=9,94, n_dfk,32
acc: 34.04 ; MSE:1.04, MAE:0.79, RMSE:1.02, r2score:-0.49,
0ky0:1.00
```

运行结果表明，在 1%的精度下，模型准确度为 34%。

第 9 段代码如下：

```
#9
print('\n\9, 绘制对比预测函数图')
df_test[['close_next','close_pred']].plot(linewidth 3)
```