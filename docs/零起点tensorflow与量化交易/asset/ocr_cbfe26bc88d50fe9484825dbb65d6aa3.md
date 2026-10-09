tq_pools_chk的代码如下：

```
```python
def tq_pools_chk(qx):
    print('\\n#tq_pools_chk,xcode=',qx.wrkStkCod)
    print(qx.wrkStkDat.tail())
```
```

如图 9-3 所示是部分输出信息。

![](bbox=[0.16, 0.26, 0.83, 0.55])

第 7.2、7.3 组代码如下：

```
```python
print('\\n#7.2,plot_inx -->tmp/tmp_.html')
xinx_df=qx.wrkInxCod,qx.wrkInxDat
hdr,fss='K线图-inx '+xinx,tmp/tmp_'+xinx+'.html'
df2=df.tail(100)
zdr,drDF_cdl(df2,etq=fss,m_title=hdr)

print('\\n#7.3,plot stk-->tmp/tmp_.html')
xcod_df=qx.wrkStkCod,qx.wrkStkDat
hdr,fss='K线图-stk '+xcod,tmp/tmp_'+xcod+'.html'
df2=df.tail(100)
zdr,drDF_cdl(df2,etq=fss,m_title=hdr)
```
```

调用集成的 Ploity 函数,绘制大盘指数 K 线图和股票数据 K 线图,分别如图 9-4 和图 9-5 所示。