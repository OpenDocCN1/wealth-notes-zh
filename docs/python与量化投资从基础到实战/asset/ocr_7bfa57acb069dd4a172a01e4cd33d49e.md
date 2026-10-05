![](bbox=[0.26, 0.11, 0.71, 0.36])

# 4. recall_score（召回率）

召回率为将提取出正确的信息条数除以样本总信息条数的计算结果，即有多少样本被正确识别。其使用方法如下：

```
metrics.recall_score(y_true, y_pred, labels=None, pos_label=1, average='binary', sample_weight=None)
```

其中，y_true 为测试集的真实类别，y_pred 为测试集的预测类别。average 参数适用于多分类问题，可取值 macro、micro、samples、wighedted 和 None。macro 为每个分类给出相同的权重，从而计算二分类 metrics 的均值；micro 给定每个样本类权重来计算整个份额；samples 通过计算真实类和预测类的差异的 metrics 来求平均值；weighted 对不均衡数量的类计算二分类 metrics 的平均值；None 将返回一个数组，包含每个类的得分。

示例如下：

```
In [183]:from sklearn.metrics import recall_score
    y_true = [0, 1, 2, 0, 1, 2]
    y_pred = [0, 2, 1, 0, 0, 1]
    recall_score(y_true, y_pred, average='macro')
Out[183]:
    0.33...
In [184]:recall_score(y_true, y_pred, average='micro')
Out[184]:
    0.33...
In [185]:recall_score(y_true, y_pred, average='weighted')
```