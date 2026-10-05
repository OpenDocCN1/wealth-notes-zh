# Python 机器学习与量化投资

的 K 近邻算法是一种基于相似分析的算法模型，可以用于价格预测。

案例 10-1 的文件名是 kb1001_knn_Reg.py。本案例是介绍 sklearn 拟决定中 K 近邻回归的 KNeighborsRegressor 的具体应用，

案例数据源采用的是 BTC 实盘交易数据，为以后大家结合实盘操作，引入数据、训练数据以及标准 OHLC 仓位数据。

前面几组代码，用于读取数据，和前面的案例 9-2 几乎小异，故此处不再重复说明。

第 5 组代码如下：

```
#5
print(' #5, 导入模型')
mx = KNeighborsRegressor()
调用 K 近邻回归的类 KNeighborsRegressor 生成模型，并保存交易 mx 当中。
```

第 6 组代码如下：

```
#6
print(' #6, fit 训练模型')
mx.fit(x,y)
调用 fit 函数训练模型。
```

第 7 组代码如下：

```
#7
print(' #7, predict 预测模型')
df_test['close_pred'] = mx.predict(xtst)
zt.prf('df', df_test)
模型训练完成后，调用模型内建的 predict 预测函数，生成验证数据。
```

第 8 组代码如下：

```
#8
print(' #8, 绘制对比走势图')
df_test[['close_next','close_pred']].plot(linewidth=3)
```

第 9 组代码如下：