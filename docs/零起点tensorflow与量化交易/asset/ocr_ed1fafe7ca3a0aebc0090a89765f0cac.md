预测的股票趋势数据进行分析评估。

下面我们对案例程序的主流程代码分组进行讲解。

第 1 组代码如下：

```
<pre>
#1
print('\n#1, set.sys')
pd.set_option('display.width', 450)
pd.set_option('display.float_format', zt.xfloat3)
</pre>
```

设置运行环境。

第 2 组代码如下：

```
<pre>
#2
print('\n#2, 读取数据')
fss='data/df_rnn020.csv'
df=pd.read_csv(fss)
zt.prx('df', df, tail())
</pre>
```

读取预测的结果数据。我们将案例 7-3 的预测结果数据文件改名为df_rnn020.csv，并保存在 data 数据目录中，便于大家学习。

如图 7-12 所示是第 2 组代码对应的输出信息。

<table>
<tr>
<td>y</td>
<td>price</td>
<td>price_next</td>
<td>price_change</td>
<td>ktype</td>
<td>y_pred</td>
</tr>
<tr>
<td>3.000</td>
<td>3.280</td>
<td>3.310</td>
<td>100.915</td>
<td>3</td>
<td>3</td>
</tr>
<tr>
<td>3.000</td>
<td>3.290</td>
<td>3.310</td>
<td>100.608</td>
<td>3</td>
<td>3</td>
</tr>
<tr>
<td>3.000</td>
<td>3.310</td>
<td>3.310</td>
<td>100.000</td>
<td>2</td>
<td>3</td>
</tr>
<tr>
<td>2.000</td>
<td>3.310</td>
<td>3.200</td>
<td>99.098</td>
<td>2</td>
<td>3</td>
</tr>
<tr>
<td>2.000</td>
<td>3.300</td>
<td>3.290</td>
<td>99.697</td>
<td>2</td>
<td>3</td>
</tr>
</table>

图 7-12 预测结果数据

第 3 组代码如下：

```
<pre>
#3
print('\n#3 准确度分析')
print('\nky0=10')
dacc, dfx, a10 = ztq.ai_acc_xed2ext(df, y, df.y_pred, ky0=10, fgDebug=True)
print('\nky0=5')
dacc, dfx, a10 = ztq.ai_acc_xed2ext(df, y, df.y_pred, ky0=5, fgDebug=True)
</pre>
```

准确度（acc）分析，分别采用 10% 和 5% 的宽度测试预测结果的准确度。其

· 215 ·