df_test(['close_next','close_pred']).plot(linewidth=3)
print(predicted)
第 9 代码如下：
:9
print('9,袋1 预测准确率!')
dicc,df,xlat zai.al_acc_xed2ext(df_test['close next'],df test ['close_pred'],i,True)
验证 BaggingClassifier (袋袋分类) 预测模型的预测结果，对应的输出如下：
9,袋1 预测准确率
ky0-1; n_d:9,113,n_d:k,56
acc: 49.56
这些结果大约在 1%的精度下，准确度是 49.56%.
第 10 代码如下：
:10
print('\n10,value_counts')
print("\ndf_test['close_next'].value_counts()")
print(df_test['close_next'].value_counts())
print("\ndf_test['close_pred'].value counts()")
print(df test['close_pred'].value_counts())
本代码是对分类预测的结果增加代码，使用 pandas 的 value_counts 统计功能。统计 close_next (实际收盘价数据)、close_pred (预测收盘价数据) 的具体数据分布情况
对应的输出信息：
:10,value counts
df_test['close_next'].value_counts()
2  51
1  51
0  9
Name: close_next, dtype: int64