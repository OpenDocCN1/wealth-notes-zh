
3.1
print('3.1,cut data')
tim0st,tim9st = '2010-10-10','2017-12-31'
df_train = xdf.loc[xdf['time'].between(tim0st,tim9st)]
zt.prDF('3.1 df_train\n',df_train)

3.2
df_test = xdf[xdf.index>'2016'].tail(100)
zt.prDF('3.2 df_test\n',df_test)

4
print('4,准备数据')
clst ['open','high','low','close']
x=df_train[clst].values
y=df_train['y'].values
xtest=df_test[clst].values

第5代码如下：

5
print('5,建立模型')
mx = tree.DecisionTreeRegressor(max_depth 3)

使用决策树回归模型生成模型,并保存在变量mx中,其中max_depth 参数为3,表示决策树的深度为3。

这个深度仅仅有些,只作为教学案例使用,为了方便决策树模型的输出。在实际当中,往往会使用较大的深度

不过决策树方法容易出现过拟合的问题,所以也很少使用类似深度学习及神经网络方法般的深度进行训练。

第6代码如下：

6
print('6,fit 训练模型')

# Python 机器学习与二化投资

mx.fit(x,y)

按照此，调用后再训练模型。

第7代码如下：

17
print('17,导出图形')
dot_data = StringIO()
tree.export_graphviz(mx, out_file=dot_data,
    feature_names=clst,
    class_names=vlst,
    filled=True, rounded=True,
    special_characters=True)
graph = pydot.graph_from_dot_data(dot_data.getvalue())
graph.write_pdf('tmp/tree01.pdf')
graph.write_png('tmp/tree01.png')

这段代码足以让案例中少有的：

机器学习模型的可是化是非常有价值的，但从模型论和的可是化以外只 有很少的模型能够实现可视化。所以 TensorFlow 深度学习平台的 TensorBoard 被人称为：众用的神经网络可视化神器。

以上代码的决策树模型可视化实际上是通过调用第三方绘图软件 Graphviz 完成的，所以在运行前，读者已先下载、安装好相关的软件。并在 Windows 环境中配好 path 系统路径参数。

第 8 代码如下：

18
print('18,predict 模型预测结果')
df_test['y2']=mx.predict(x_tst) #cross_val_predict(mx,x,y,
cv=20)
zt.prDF('df',df_test)
df_test([['y','y2']].plot(linewidth=3)
print(predicted)

模型训练完成后，利用模型内的 predict 方法生成验证数据，并绘制对比图。

# 第 9 节代码如下：

#9
print("\n9,验证模型预测结果")
#9.1
print("\n9.1,按 5% 样本验证")
dacc,df_zai.ai_acc_xed2x(df_test['y'], df_test('y2'], 0,True)
print('acc',dacc)

#9.2
print("\n9.2,按 1% 样本验证")
dacc,df,xlost_zai.ai_acc_xrd2ext(df_test('y'],df_test('y2'],1, True)

验证决策树的模型结果。

表 6.1 是调用决策树回归函数时，max_depth 参数分别为不同值的运行结果。

# 表 6.1 运行结果

| max_depth 值 | 准确度，按 1% 样本 | 准确度，按 5% 样本 |
|---|---|---|
| 3 | 9.57% | 70.2% |
| 5 | 28.72% | 100% |
| 30 | 52.13% | 100% |
| 50 | 51.09% | 100% |

验证结果表明，50 层的决策树的准确度比 30 层的决策树的准确度还低。这也是决策树模型一种过拟合的现象。

图 6.5～图 6.8 所示是对应不同 max_depth 决策树深度参数时，训练数据与实际数据的对比图。图中，曲线 y 是真实数据，曲线 y2 是决策树生成的预测数据。

# Python 机器学习与量化投资

![](img/93734a9b810ff3e8778504a648b013dc_127_0.png)

图 6.5 决策树模型对比图，决策树深度：3 层

![](img/93734a9b810ff3e8778504a648b013dc_127_1.png)

图 6.6 决策树模型对比图，决策树深度：5 层

图 6.7 决策树模型对比图，决策树深度：30层

图 6.8 决策树模型对比图，决策树深度：50层

## 6.3 决策树分类参数

DecisionTreeClassifier 接口位于决策树模块，函数调用接口如下：

```
DecisionTreeClassifier(criterion='gini', splitter='best',
```

# Python机器学习与量化投资

```
max_depth=None, min_samples_split 2, min_samples_leaf 1,
min_weight_fraction_leaf=0.0, max_features=None, random_state=None,
max_leaf_nodes=None, min_impurity_split=1e-07, class_weight None,
presort=False
```

以上接口参数看起来很多，其实常用的一个参数（max_depth 参数，即决策树的深度

其他参数常用的参数与 DecisionTreeRegressor 函数的接口类似，这里不作叙述

## 案例 6-2：决策树分类算法

案例 6-2 的文件名是 kb602_treeClass.py，本案例介绍的是 sklearn 扩展库中决策树分类函数 DecisionTreeClassifier 的使用方法。

前面 4 组代码都是数据准备，如下所示：

```
#1
print('#1, read data')
?ss 'data/sh2013.csv'
xdf=pd.read_csv(fcs, index_col=0)
xdf=xdf.sort_index()
zt.prDF('xdf',xdf)
```

```
#2
print('#2, xcd data')
xdf['xprice']=xdf['close'].shift(-1)
xdf['y']=xdf['xprice']
...
xdf['kpr']=xdf['xprice']/xdf['close']*1000
xdf['y']=0
xdf.loc[xdf.kpr>1005,'y']=2
xdf.loc[xdf.kpr<995,'y']=-1
xdf.fillna(method='pad',inplace True)
zt.prDF('xdf',xdf)
```

3.1
print('3},cut data')
tim0str,tim9str='2010-10-10','20:7-12-3'
df_train-zd it.df_xcut0tim(xdf,'',tim0str,tim9str)
zt.prDF('3.1 df_train\n',di_train)

3.2
df_test xdf[xdf.index>'2018'] .tail(200)
zt.prDF('3.2 df_test\n',df_test)

4
print('4,沪深AI实战!')
clst=['open','high','low','close']
xdf_train[clst].values
y=df_train['y'].values
xtst=df_test[clst].values

数据准备阶段，因为是分类似型，所以需要先把数据替换为分类类型，这个步骤在第2组代码中完成：

```
xdf['kpr']=xdf['xprice']/xdf['close']*1000
xdf['y']=0
xdf.loc[xdf.kpr>1005,'y']=2
xdf.loc[xdf.kpr<995,'y']=1
```

因为股票市场波动很小，我们按收盘价千分之五的上下波动对数据进行分类：

+   - 2，波动大于100.5%，up上涨似式。
- 1，波动小于99.5%，down下跌似式
- 0，波动在0.5%之间，eq平衡似式。

以上分类类似足彩小样的“3、1、0，正、平、负”似式，稍后第7组代码的走势标签vlst会在图6.9中说明：

# Python 机器学习与量化投资

```
v1st=['up','down','eq']
```

第 5 迭代如下：

```
#5
print('#5, 创建模型')
rx = tree.DecisionTreeRegressor(max_depth=3)
$x = tree.DecisionTreeClassifier(max_depth=3)
```

调用 DecisionTreeClassifier 模型，生成模型，并保存至变量 mx 中。
调用参数 max_depth 为 3，表示决策树的深度只有 3 层。

第 6 迭代对模型进行训练，具体如下：

```
#6
print('#6, fit 训练模型')
mx.fit(x,y)
```

第 7 迭代代码输出模型结构，具体如下：

```
#7
print('#7, 绘制模型')
v1st=['up','down','eq']
dot_data = StringIO()
tree.export_graphviz(mx, out_file dot_data,
    feature_names=clst,
    class_names v1st,
    filled=True, rounded=True,
    special_characters=True)
graph = pydot.graph_from_dot_data(dot_data.getvalue())
graph.write_pdf('tmp/tree01.pdf')
graph.write_png('tmp/tree02.png')
```

为了进一步说明模型内部的分类情况，我们增加了一组 v1st 分类列表变...

图 6.9 是对应的决策树分类的模型。

![](img/93734a9b810ff3e8778504a648b013dc_132_0.png)

图 6.9 决策树分类方法模型

# Python 机器学习与量化投资

图 6.9 中每个节点下面的“class=xx”就是分类的思想，对应 vlist 中定义的类别。

此外图 6.9 绘图节点中的基因指数和 vlist 对应的特征字段都可以用于绘图的分析、优化。

第 8 组代码如下：

```
8
print('8,predict 预测结果')
df_test['y2']=mx.predict(xtst) #cross_val_predict(mx,x,y,cv=20)
zt.plot('df',df_test)
```

另外训练完成后，可用模型内置的 predict 函数，生成预测结果，并绘制对比图：

第 9 组代码如下：

```
9
print('9,准确率对比结果')
9.1
print('\n#9.1,按5折交叉验证')
dacc,df_zai.ai_acc_xed2x(df_test['y'],df_test['y2'],5,True)
print('acc',dacc)
9.2
print('\n#9.2,按1折交叉验证')
dacc,df_xlst_zai.ai_acc_xed2ext(df_test['y'],df_test['y2'],1,True)
9.3
print('\n#9.3,value_counts')
print("\ndf_train['y'].value_counts()")
print(df_train['y'].value_counts())
print("\ndf_test['y'].value_counts()")
```

```
print(df_test['y'].value_counts())
print("\n df_test['y2'].value_counts()")
print(df_test['y2'].value_counts())
```

论证决策树的效果结果

表 6.2 使用 DecisionTreeRegressor 时，max_depth 参数分别为不同的运行结果

# 表 6.2 运行结果

| max_depth 参数 | 训练集、测试15%误差 | 测试集、测试5%误差 |
|---|---|---|
| 3 | 41.49 | 41.489 |
| 5 | 41.49 | 41.489 |
| 10 | 35.11 | 35.106 |
| 30 | 38.50 | 38.298 |

此外，代码还输出了不同决策树参数 max_depth，对应在预测数据字段 y2 的结果中，以及在不同分类数据的 value_counts 统计结果中

## 6.4 GBDT算法

GBDT 算法英文全称是 Gradient Boosting Decision Tree，简称 GBDT 算法。

GBDT 算法是一个集成模型，也是一个集成学习算法。简单来说，GBDT 算法是指用许多贪义决策树算法组成了一个更强大的学习算法。

GBDT 算法由多棵决策树组成，将所有树的结论累加起来作为最终结果 它在提出之初和 SVM 一起被认为泛化能力较强的算法 近几年更因为被用于搜索排序的机器学习模型而引起大家关注，如图 6.10 所示。

GBDT 算法是一个应用很广泛的算法，可以用来做分类和回归，在很多的数据上都有不错的效果。GBDT 这个算法还有一些其他的名子，比如 MART（Multiple Additive Regression Tree）、GBRT（Gradient Boosting Regression Tree）、Tree Net 等。

![](img/93734a9b810ff3e8778504a648b013dc_135_0.png)

图 6.10 GBDT 方法

## 6.5 迭代决策树函数

sklearn 模块内的迭代决策树函数包括两个：

+   - GradientBoostingRegressor，迭代回归决策树函数
- GradientBoostingClassifier，迭代分类决策树函数

两个函数都位于 Ensemble（集成）方法模块，这也说明 GBDT 方法是一个集成学习方法

迭代回归决策树函数的调用接口如下：

```
GradientBoostingRegressor(loss='ls', learning_rate=0.1,
 n_estimators 100, subsample=1.0, criterion='friedman_mse',
 min_samples_split=2, min_samples_leaf 1, max_depth 3,
 min_weight_fraction_leaf=0.0, min_impurity_decrease=0.0,
 min_impurity_split None, init None, random_state None,
 max_features None, alpha=0.9, verbose=0, max_leaf_nodes=None,
 warm_start=False, presort='auto')
```

迭代分类决策树函数的调用接口如下：

```
GradientBoostingClassifier(loss='deviance', learning_rate=0.1,
 n_estimators=100, subsample=1.0, criterion='friedman_mse',
 min_samples_split 2, min_samples_leaf 1, max_depth 3,
 min_weight_fraction_leaf=0.0, min_impurity_decrease=0.0,
 min_impurity_split=None, init=None, random_state=None,
 max_features None, verbose 0, max_leaf_nodes None,
 warm_start=False, presort='auto')
```

两个函数的调用接口基本相同，主要参数如下。

+   - n_estimators: 梯度提升的迭代次数，也是弱分类器的个数，默认值为 100 次。 
- loss: 损失函数，包括 "ls" "lad" "huber" "quantile"，默认值为 "ls"。 
- learning_rate: 学习步长，默认值为 0.1。 
- max_depth: 决策树最大深度，默认值为 3。 
- warm_start: 如果是 True，会在之前的拟合结果以 Деятельность 迭代次数，默认值为 False。

由以上参数可以看出，GBDT 默认已经包括了神经网络的不少特点，参数 learning_rate、loss 与神经网络模型中完全相同。如果结合 BP 反馈式使用，构成一个简单的神经网络模型。

## 案例 6-3： GBDT 回归算法

案例 6-3 的文件名是 kb603_GBDT_reg.py，本案例介绍的是 GBDT 回归算法的应用，核心代码如下

前面 4 组代码用于数据准备，代码如下所示：

```
print('1, read data')
 fss = 'data/sh2016.csv'
 xdf = pd.read_csv(fss, index_col=0)
 xdf = xdf.sort_index()
 zt.pdf('xdf', xdf)

 print('2, xed data')
 xdf['xprice'] = xdf['close'].shift(-1)
 xdf['y'] = xdf['xprice']
 xdf.fillna(method='p.d', inplace=True)
```
