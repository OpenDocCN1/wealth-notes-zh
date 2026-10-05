```
03
print('\n3. 定权')
mx = zai.mx_line(x_train.values, y_train.values)
```

本案例中第 3 组代的使用的线性回归拟合建立机器学习模型，
在此并没有直接使用 sklearn 模块中的 LinearRegression 线性回归线性模型，而是通过 ztools_ai 提供的高能模块的 mx_line 函数对它进行调用的，对应的函数代码是：

```
def mx_line(train_x, train_y):
    rx = LinearRegression()
    rx.fit(train_x, train_y)
    print('\nlinreg.intercept_')
    print (mx.intercept_); print (mx.coef_)
    return mx
```

线性回归函数，位于 sklearn.linear_model 模块中，的接口是：
LinearRegression(fit_intercept=True, normalize=False,
copy_X=True, n_jobs=1)

mx_line 函数代码很简约，调用基于最小二乘法的 LinearRegression 线性回归函数，生成模型变量 mx，运行内置的 fit 命令，分析学习训练数据：train_x（训练数据）、train_y（训练数据对应的答案）。

sklearn 模块库中的各种机器学习函数，基本上都采用 fit 命令进行学习、建立模型。

采用 ztools_ai 扩展智能模块的 mx_xxx 系列函数，对 sklearn 模块中的各科智能化函数进行二次封装，其优点如下

+   • 统一调用接口，规范函数 API 调用模式

+   • 无需主攻面对底层机器学习函数，如 LinearRegression，无需了解其相关的理论知识即可止去使用。

如果不为记各各义尔的机器学习函数名称，只要记住 mx_xxx 系列函数，诸多义尔的机器学习模型过四，就基本简化成一个简单的 fit 内建的步骤了