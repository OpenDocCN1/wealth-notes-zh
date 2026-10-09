```
plt.plot(f_te, p_ols, '-', label='OLS prediction')
plt.plot(f_te, p_dnn, '--', label='DNN prediction')
plt.legend();
```

![](bbox=[0.16, 0.16, 0.85, 0.46])

# ## 5.6 偏差和方差

一般来说，机器学习中的一个主要问题是过拟合，特别是当机器学习算法应用于金融数据时。当验证数据集和测试数据集的性能比训练数据集的性能差时，就是模型过度拟合了其训练数据，使用 OLS 回归的示例可以在视觉上和数字上说明这个问题。

下面的 Python 代码使用较小的子集进行训练和验证，并实现了线性回归和高阶回归。如图 5-12 所示，线性回归拟合在训练数据集上有较高的偏差，预测数据和标签数据之间的绝对差异相对较大。高阶拟合显示出了较高的方差，它精确地命中所有训练数据点，但为实现完美拟合，拟合本身变化会很大。

```
In [57]: f_tr = ff[:20:2] 1
 l_tr = ll[:20:2] 1
In [58]: f_va = ff[1:20:2] 2
 l_va = ll[1:20:2] 2
In [59]: reg_b = np.polyfit(f_tr, l_tr, deg=1) 3
In [60]: reg_9 = np.polyfit(f_tr, l_tr, deg=9, full=True)[0] 4
In [61]: f_ = np.linspace(f_tr.min(), f_va.max(), 75) 5
In [62]: plt.figure(figsize=(10, 6))
 plt.plot(f_tr, l_tr, 'ro', label='training data')
```