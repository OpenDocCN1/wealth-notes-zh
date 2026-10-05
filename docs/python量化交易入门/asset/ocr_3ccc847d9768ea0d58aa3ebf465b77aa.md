```
# 第3步: 训练模型
# fit()函数, 传入第一个参数是训练数据的特征 x, 第二个参数是训练参数的标签 y
model.fit(X_train, y_train)
>> LinearRegression(copy_X=True, fit_intercept=True, n_jobs=1, normalize=False)
```

（3）建立最佳拟合线。最佳拟合线为 z = a + x，其中，截距为 a；回归系数为 b。

```
# 建立最佳拟合线
a = model.intercept_
b = model.coef_
print ('最佳拟合线, 截距 a=', a, ', 回归系数 b=', b')
>> 最佳拟合线, 截距 a= [7.38860355], 回归系数 b= [[16.42445973]]
```

# 4. 绘制最佳拟合线

通过绘图库 Matplotlib 来绘制最佳拟合线，如图 2-12 所示。

```
import matplotlib.pyplot as plt
#训练数据散点图
plt.scatter(X_train, y_train, color='b',label='train data')
# 训练数据的预测 (通过机器学习 sklearn 中的 Linear Regression, 用 model.predict )
y_train_pred = model.predict(X_train)
#绘制最佳拟合线图,线而不是散点, 不用 scatter, 而用 plot
plt.plot(X_train, y_train_pred, color='black', linewidth='3', label='best line')
# 添加图标标签
plt.legend(loc=2)
plt.xlabel('Hours')
plt.ylabel('Score')
#显示图函数 show()
plt.show()
```