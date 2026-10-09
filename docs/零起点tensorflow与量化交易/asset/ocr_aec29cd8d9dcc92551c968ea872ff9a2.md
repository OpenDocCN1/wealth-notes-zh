| 3373 云南 600806 *STKJ_600806 600806 33 | 11 | 机床制造 *ST 昆机 |
|---|---|---|
| 3374 吉林 600432 *STJE_600432 600432 42 | 33 | 小金属 *ST 君恩 |
| 3375 浙江 300702 TYGF_300702 300702 397 | 80 | 化学制药 天守股份 |
| 3376 河南 300701 SBGF_300701 300701 77 | 60 | 电器仪表 森霸股份 |

注意：以上数据列排序有些混乱，需要进一步优化。

第 4 组代码如下：

```
```python
#4
fss='tmp/xstk_name.csv'
xlst=['code','name','ename','id','industry','id_industry','area','id_area']
print('\n#4,fss,',fss)
print('xlst,',xlst)
df3=pd.DataFrame()
df3=df2[xlst]
df3.to_csv(fss,index=False,encoding='GBK')

print('\ndf3.head()')
print(df3.head())
#
print('\ndf3.tail()')
print(df3.tail())
```
```

优化数据列排序，保存数据文件。其对应的输出信息如下：

```
```
#4,fss, tmp/xstk_name.csv
xlst, ['code', 'name', 'ename', 'id', 'industry', 'id_industry', 'area', 'id_area']

df3.head
        code    name      ename    id industry id_industry area id_area
0   603882  西藏城投    MUC_603882  603882  医疗保健          63  广东    283
1   300695   N汽车    NZF_300695  300695  汽车配件         113  浙江    397
2   300318  博晖创新    BBXC_300318  300318  医疗保健          63  北京    302
3   000815   美利云    MLY_000815  000815  造纸           23  宁夏     13
4   002680  长生生物    CSSW_002680  2680  生物制药          41  江苏    364

df3.tail
        code    name      ename    id industry id_industry area id_area
3372  603055  台华新材    THXC_603055  603055  纺织           47  浙江    397
```
```