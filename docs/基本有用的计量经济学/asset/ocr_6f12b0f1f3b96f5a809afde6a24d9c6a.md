下面是利用 pstest 命令检测变量 age、re75 匹配前后的平衡性：

```
. pstest age re75. both
```

| Variable | Unmatched Matched | Mean Treated Control | % reduct %bias |bias| | t-test t p>|t| | V(T)/V(C) |
| age | U | 25.816 | 25.054 | 10.7 |  | 1.12 | 0.265 | 1.03 |
|  | M | 25.816 | 26.568 | -10.6 | 1.4 | -0.92 | 0.356 | 0.88 |
| re75 | U | 1532.1 | 1266.9 | 8.4 |  | 0.87 | 0.382 | 1.08 |
|  | M | 1532.1 | 1635.7 | -3.3 | 60.9 | -0.27 | 0.784 | 0.77 |
* if variance ratio outside [0.75; 1.34] for U and [0.75: 1.34] for M

第 1 列是变量名，第 2 列对应每个变量有两行，U 表示匹配前原始样本，M 表示匹配样本，第 3、4 列分别是干预组和控制组的变量平均值，第 5 列为标准化平均值差异，以百分数形式表示，第 6 列为匹配后标准化平均值差异下降的幅度，最后—列为协变量方差比，中间提供了 t 检验值和相应的 p 值。另外，可以用 ps graph 画出两组的倾向指数分布图。

# 3. 培训的影响：倾向指数匹配方法案例

下面用 Dehejia and Wahba（1999）的随机化实验干预组样本（nsw_dw.dta）和 CPS 调查数据（cps_controls.dta）合并为一个数据集，利用倾向指数匹配方法，估计干预组平均因果效应。

首先构造数据集：

```
use nsw_dw,clear // 打开随机化实验数据
drop if treat == 0 // 删除实验数据的控制组
append using cps_controls // 将 CPS 数据追加上来
save nsw_cps, replace // 将新数据保存为 nsw_cps.dta
```

数据构造完成后，检测协变量是否平衡。先做个简单统计或平衡性检验：①

```
estpost tabstat age - re75,by(treat) s(mean sd) column(s) listwise //用 tabstat 分组简单统计
```

```
eststo sum // 结果保存到 sum 中
```

```
esttab sum, main(mean) aux(sd) unstack nonumber nomtitle nogap // 将统计结果列到显示器上
```

> ① estpost、eststo、esttab 等是用户写的命令。需要先安装才可以使用。具体参考本书附录。