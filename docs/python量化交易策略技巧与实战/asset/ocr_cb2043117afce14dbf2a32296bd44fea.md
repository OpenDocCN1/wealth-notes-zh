```
# -2 是保证获取的数据是昨天的，-1 就是通过今天的数据计算出来的指标
ema_data = talib.EMA(close_prices)[-2]  #指数移动平均线指标
rsi_data = talib.RSI(close_prices)[-2]  #RSI 指标
features = []
#向列表变量中添加指数移动平均线指标和 RSI 指标
features.append(ema_data)
features.append(rsi_data)
#设置变量 label 为布尔变量，并赋值为 False
label = False
#如果今天收盘价大于昨天收盘价，则变量 label 为 True
if close_prices[-1] > close_prices[-2]:
    label = True
x_all.append(features)
y_all.append(label)
```

最后准备 SVM 算法需要用到的数据，并显示提示信息，具体代码如下：

```
x_train = x_all[:-1]
y_train = y_all[:-1]
x_test = x_all[-1]
y_test = y_all[-1]
print('支持向量机(SVM)数据已准备好了！')
```

单击工具栏中的 ▶ 运行 按钮，运行结果如图 12.10 所示。

![](bbox=[0.15, 0.534, 0.9, 0.846])

# 图 12.10 支持向量机（SVM）数据准备代码

接下来导入 SVM 分类器，训练样本的特征是根据 2016 年 3 月 2 日至 2019 年 1 月 4 日每一