```
print('\n#3,fss, ',fss)
df2.to_csv(fss,index=False,encoding='GBK')
#
print('\ndf2.tail()')
print(df2.tail())
```

在第 3 组代码中，如下代码把 id 字段强行设置为 int 格式，否则会是浮点数格式。

```
df2['id']=df2['id'].astype(int)
```

第 3 组代码对应的输出信息如下：

```
#3,fss, tmp/xlnx_code.csv
df2.tail
code ename id name tim0
19 399106 SZZL_399106 19 深证综指 2000-01-01
20 399107 SZA2_399107 20 深证A指 2000-01-01
21 399108 SZBZ_399108 21 深证B指 2000-01-01
22 399333 ZXBR_399333 22 中小板R 2007-01-01
23 399606 CYBR_399606 23 创业板R 2011-01-01
```

# 案例 3-6：AI 版索引数据库

案例 3-6 文件名为 kc306_aiName02.py，主要介绍如何修改股票代码文件 stk_code.csv，生成一个增强版本的股票数据文件。

因为股票代码文件的数据格式比指数代码文件多，所以增强版本的股票数据文件所增加的数据也比较多，如下所示。

+   • ename，指数的名称的拼音缩写。

+   • id，指数名称的数字 id。

+   • id_industry，行业分类的数字 id。

+   • id_area，地区分类的数字 id。

下面我们对案例程序进行分组讲解。

第 1 组代码如下：

```
#1
fss='inxs/stk_code.csv'
```