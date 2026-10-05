```
special_characters=True
)
graph = pydot.graph_from_dot_data(dot_data.getvalue())
graph.write_pdf('tmp/tree01.pdf')
graph.write_png('tmp/tree01.png')
...
```

虽然是决策树方法，但此山于足多种决策树的集成模式，所以无法绘出决策内部的层次结构。

第8代码如下：

```
#8
print('8,predict 模型实际结果')
df_test['y2'] = x.predict(xtst) #cross_val_predict(mx,x, y, cv=20)
zt.plot('df',df_test)
df_test[['y','y2']].plot(linewidth 3)
#print(predicted)
```

模型训练完成后，调用模型内部的 predict 预测函数，生成验证数据。

第9代码如下：

```
#9
print('9,验证预测结果')
#9.1
print('\n9.1,按5折交叉验证模型')
dacc,df=zal.ai_acc_xed2x(df_test['y'],df_test['y2'],5,True)
print('acc',dacc)
#9.2
print('\n9.2,按1折交叉验证模型')
dacc,df,xlst=zal.ai_acc_xed2ext(df_test['y'],df_test['y2'],1,True)
```

验证决策树的预测结果：

在6.3处调用GradientBoostingRegressor函数时，n_estimators 参数分别