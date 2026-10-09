```
print(df9.tail())
print(ftg_rnd)
df9.to_csv(ftg_rnd,index=False)
```

深度学习、神经网络系统在训练模型时，往需要打乱数据排序，采用随机序列，以避免相邻数据间的干扰。

TDS 金融数据集本身就对数据进行了随机化处理，在编写神经网络程序时，可以减少一些数据预处理的环节。

这样做的缺点是，对于少数基于时序的神经网络模型，丢失了时序数据，需要额外选择数据集。目前这方面的应用主要是 NLP 语义分析，金融数据应用相对较少，权衡比较后，TDS 还是采用了随机序列的数据模式。

在程序中，实际执行随机命令的是如下代码：

```
df9=df9.sample(frac=1.0)
```

通过 Pandas 内置的 sample 随机取样函数，对原始数据进行随机排序处理。

第 4 组代码如下：

```
#4 cut:trian,test
n9=len(df9.index)
num_train,num_test=round(n9*0.9),round(n9*0.1)
df_train=df9[0:num_train]
df_test=df9.tail(num_test)
#
print('\n'+'df_train.tail()' + '\n')
print(df_train.tail())
df_train.to_csv(ftg_train,index=False)

print('\n'+'df_test.tail()' + '\n')
print(df_test.tail())
df_test.to_csv(ftg_test,index=False)
```

在深度学习、神经网络模型中，往往把数据分为训练数据集和测试数据集。以上程序就是对数据进行分割，这里采用 9:1 的比例，即 90% 的数据用于模型学习训练，10% 的数据用于测试。

本案例的数据文件全部保存在 tmp 目录下，程序运行完成后，会在 tmp 目录下生成以下几个文件。