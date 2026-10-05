```
symbols = get_industry("I65")
df = pd.DataFrame()

for symbol in symbols:
    try:
        data = history_n(symbol=symbol,frequency="1d",count=42,end_time="2018-12-30",fields="close",fill_missing="last",adjust=ADJUST_PREV,df=True)
        data_close = pd.DataFrame({symbol:data["close"]})
        if len(df) == 0:
            df = (data_close)
        else:df = pd.concat([df,data_close], axis=1)
    except:pass

corr = df.corr()

print ('相关系数大于 0.9 的组合对数有' +str( (np.sum(corr.values.ravel()>0.9) - len(symbols))/2)+ '对')
print ('相关系数大于 0.95 的组合对数有' +str( (np.sum(corr.values.ravel()>0.95) - len(symbols))/2)+ '对')

print(corr[corr>0.95])
```

从【程序 11-9】中可以看到，首先从掘金量化数据库中读取了其对应的数据，之后按 DataFrame 生成对应的数据格式，使用生成的 DataFrame 数据集自带的数据相关性计算数据集内部的数据相关性。

打印结果如下：

相关系数大于 0.9 的组合对数有 2050.0 对

相关系数大于 0.95 的组合对数有 601.0 对

需要说明的是，这里为了演示配对程序，选择了软件和信息技术服务行业，此行业有 197 只股票代码，计算其不同的相关性。代码 print (corr[corr>0.95])打印出了所有相关系数大于 0.95 的股票，结果如图 11-6 所示。

|  | SHSE.600289 | SHSE.600536 | SHSE.600556 | SHSE.600571 | SHSE.600588 |
| SHSE.600289 | 1.0000 | NaN | NaN | NaN | NaN |
| SHSE.600536 | NaN | 1.0000 | NaN | NaN | NaN |
| SHSE.600556 | NaN | NaN | 1.000 | NaN | NaN |
| SHSE.600571 | NaN | NaN | NaN | 1.0000 | NaN |
| SHSE.600588 | NaN | NaN | NaN | NaN | 1.0000 |
| SHSE.600602 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600654 | 0.9503 | NaN | NaN | NaN | NaN |
| SHSE.600701 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600718 | NaN | 0.9548 | NaN | NaN | NaN |
| SHSE.600728 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600892 | NaN | NaN | NaN | NaN | NaN |
| SHSE.900901 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600406 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600410 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600446 | NaN | NaN | NaN | NaN | NaN |
| SHSE.600476 | NaN | 0.9586 | NaN | 0.9585 | NaN |

图 11-6 相关系数大于 0.95 的部分数据