```
print('\n\nstallb', staLib)
```

因为 sta 仅仅反映和实际对应的字以 close_next 价格绝对无法直接对应比，因此需要建立一个 sta 的对照表。

这个 sta 的对照表就是变量 staLib，采用的是 Python 的字典格式。

具体转换也是调用 TOP 模型模块的 df_xpl00lst 函数，下面运行结果如下：

下：

8，用于 HMM 模型状态预测和实际对应的状态对照表

```
0 0 0 53.09 0 [53.09, 22.84, 24.07]
1 0 0 45.3 0 [45.3, 27.14, 27.56]
2 0 0 39.12 0 [39.12, 27.0, 33.88]
3 0 0 41.43 0 [41.43, 30.84, 27.73]
4 2 46.67 0 [16.67, 36.67, 46.67]
5 0 53.03 0 [53.03, 20.46, 26.51]
```

```
staLib {0: 0, 1: 0, 2: 0, 3: 0, 4: 2, 5: 0}
```

第 9 迭代如下：

```
#9
print('\n9,predict 模型预测结果')
df_test['sta_tst']=x.predict(xtst)
```

输入 test 测试数据表，生成对应的 sta 预测状态值表

第 10 代码如下：

```
#10
print('\n10,状态predict预测结果')
df_test['close_pred']=-1
for xc in stalst:
    xprdst=Lib.get(xc)
    if xprd!=None:
        print(xc,'\n',xprd)
        df_test.loc[df_test.sta_tst==xc,'close_pred']=xprd
```

根据要求，生成的 sta 预测状态数据子以通过对照 sta 的对照表变量 staLib，转换为实际的预测价格对应的字段 close_pred。