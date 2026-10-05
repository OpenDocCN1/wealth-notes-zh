调用 BaggingRegressor 函数，生成模型，并保存至变量 mx 当中。

第 6 行代码如下：

```
6
print('6, fit 训练模型')
mx.fit(x, y)
```

按例子，调用 fit 函数，训练模型。

第 7 行代码如下：

```
7
print('7, predict 预测结果')
df_test['close_pred'] = mx.predict(x_test)
zt.prfDF('df', df_test)
```

当模型训练完成后，调用模型内含的 predict 预测函数，使用测试数据生成预测数据。

第 8 行代码如下：

```
8
print('8, 绘制对比图')
df_test[['close_next','close_pred']].plot(linewidth=3)
```

第 9 行代码如下：

```
9
print('9, 检验结果并输出')
dacc, df, xlst = zi.ai_acc_xed2ext(df_test['close_next'], df_test['close_pred'], 1, True)
```

验证 BaggingRegressor 函数的预测结果，以下是对应的输出信息：

9, 检验结果并输出
ky0-1; n_df9,94, n df%,56
acc: 59.57%; MSE:2047.02, MAE:33.18, RMSE:45.24, r2score:0.89, rky0:1.00

运行结果表明在 1%的情况下，准确度为 59.57%。