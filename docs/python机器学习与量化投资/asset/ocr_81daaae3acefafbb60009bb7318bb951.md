# Python 机器学习与量化投资

```
print('01,read data')
fss 'd_t1/sh2010.csv'
xdf=pd.read_csv(fss, index_col=0)
xdf=xdf.sort_index()
zt.prDF('xdf',xdf)

#2
print('02,xcd data')
xdf['close_next']=xdf['close'].shift(-1)
xdf['xprice'] xdf['close'].shift(-1)
xdf['y'] xdf['xprice']
# 
xdf['kpr']=xdf['xprice']/xdf['close']*1000
xdf['close_next']=0
xdf.loc[xdf.kpr>1005,'close_next']=2
xdf.loc[xdf.kpr<995,'close_next']=1
xdf.fillna(method 'pad', inplace True)
zt.prDF('xdf',xdf)
)

xdf.fillna(method:'pad', inplace True)
zt.prDF('xdf',xdf)

#3.1
print('03,cut data')
tim0str,tim9str='2010-10-10','2017-12-31'
df_train=zd...df_xcut8tim(xdf,'',tim0str,tim9str)
zt.prDF('03.1 df_train\n',df_train)
#
#3.2
df_test=xdf[xdf.index>'2018'].tail(100)
zt.prDF('03.2 df_test\n',df_test)
因为是分类模型，所以事先需要对数据进行分类处理
分类后的字段名仍然用 close_next 代替了原来的字段名 y，以方便调用
mxist_DTree010 的...
```