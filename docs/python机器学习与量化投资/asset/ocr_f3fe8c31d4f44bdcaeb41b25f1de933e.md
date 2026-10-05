# 图 16.4 预测结果

第 8 组代码如下：

```
8
print('8,绘制对比图')
df_test[['close_next','close_pred']].plot(linewidth=3)
```

第 9 组代码如下：

```
9
print('9,按1.99%通过检验')
acc,df,x1,t-zui...acc_x-d2-xt(df_test['close_next'],df_test['close_pred'],1,True)
```

对于 BaggingClassifierKNN 方法模型的预测结果，对应的输出如下：

```
9,按1.99%通过检验
ky0=1; n_dE9,1:3,n_dE%,51
acc: 45.13
```

结果表明在 1% 的前提下，准确度为 49.56%。

第 10 组代码如下：

```
10
print('\n:10,value_counts')
print("\nnd1_test['close_next'].value_counts()")
print(df_test['close_next'].value_counts())
```

![](bbox=[0.07, 0.085, 0.896, 0.356])