```
7
print('7,predict 预测结果')
df_test['close_pred'] = rx.predict(xtest)
zc.plot('df', df_test)
```

第 8 行代码如下：

```
8
print('8,绘制对比图')
df_test[['close_next','close_pred']].plot(linewidth=3)
```

第 9 行代码如下：

```
9
print('9,准确率评估, 1次')
9.1
print('\n 9.1, 含 5% 误差评估')
d_acc, df = zmi.ai_acc_xcd2x(df_test['close_next'], df_test['close_pred'], 5, True)
print('acc', dacc)
```

```
9.2
print('\n9.2, 含 1% 误差评估')
dacc, df, xlst = zai.ai_acc_xcd2ext(df_test['close_next'], df_test['close_pred'], 1, True)
```

这是以小仓回归改的预测结果，以下是对应的输出信息：

9, 准确率评估结果
9.1, 含 5% 误差评估结果
acc 100

9.2, 含 1% 误差评估结果
acc: 73.4

运行结果如下。

+   - 在 5%的精度下，准门度是 100%。
- 在 1%的精度下，准门度是 73.4%。