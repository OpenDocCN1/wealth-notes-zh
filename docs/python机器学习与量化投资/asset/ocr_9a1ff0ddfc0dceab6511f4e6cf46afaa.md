# 案例 16-2：多层感知器分类算法

案例 16-2 的文件名是 kb1602_mlp_cla.py，本案例主要介绍 sklearn 模块中多层感知器分类算法的具体应用。

前面几组代码用于数据准备，和前面的案例 9-2 大同小异，故此不做说明。

因为是分类算法，所以数据准备阶段采用的是分类模式，大家注意相关代码。

第 5 组代码如下：

```
#5
print(' #5, 建立模型')
 mx = neural_network.MLPClassifier()
```

调用 MLPClassifier（多层感知器分类）的方法生成模型，并保存在变量 mx 中。

第 6 组代码如下：

```
#6
print(' #6, 训练模型')
mx.fit(x,y)
```

按惯例，调用 fit 函数，训练模型。

第 7 组代码如下：

```
#7
print(' #7, predict 模型预测结果')
df_test['close_pred'] = mx.predict(xtst)
zt.prfDF('df',df_test)
```

当模型训练完成后，调用模型内的 predict 预测函数，生成验证数据，对应的输出信息，如图 16.4 所示。