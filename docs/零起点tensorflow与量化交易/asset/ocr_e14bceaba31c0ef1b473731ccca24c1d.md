```
y_pred = model.predict(x_test)
print(y_pred)
print('type(y_pred):',type(y_pred))
```

利用已经训练好的模型进行预测。在实操操作中，一线操盘手可以直接从这个阶段开始，把前面专业、复杂的模型设计交给公司的技术部门来做，甚至在做好策略保密的前提下进行外包。

预测使用的是机器学习中经典的 predict 预测函数。其对应的输出信息如下：

```
#6 模型预测
[
[
 -58.44807144]
[
 -79.72708893]
[
 11.27677155]
[
 -48.40455627]
[
 66.74372101]

......

[
 -88.02270508]
[
 -84.58953857]
[-146.46237183]]
type(y_pred):<class 'numpy.ndarray'>
```

注意最后一条输出信息：

```
type(y_pred):<class 'numpy.ndarray'>
```

说明输出的数据变量 y_pred 的数据格式是 NumPy 的 ndarray 多维数组，这是因为 predict 预测函数支持一次输入多组数据，输出多组预测数据。

第 7 组代码如下：

```
#7 整理预测数据
print('\n#\ 整理预测数据 ')
print('\n acc.xlsx')

y2x=y_pred.flatten()[:]
print('\n y2x');print(y2x)
print('type(y2x):',type(y2x))
















ds2y=zd.at.ds4x(y2x,df_test.index)
df_test['y_pred']=ds2y
print('\ndf_test')
```