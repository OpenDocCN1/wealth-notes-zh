方、5、6列控制了出生年份、所在地区、种族、城市及婚姻状态，7、8列在5、6列基础上进一步控制年龄及其平方。使用的工具变量为所有出生季度及出生年份的交叉项。估计程序如下：

```
global YR "YR20 YR21 YR22 YR23 YR24 YR25 YR26 YR27 YR28" // 定义出生年份宏
global RG "NEWENG MIDATL ENOCENT WNO CENT SOATL ESOCENT WSOCENT MT" // 定义地区宏
global IV "QTR120 - QTR129 QTR220 - QTR229 QTR320 - QTR329 YR20 - YR28" // 定义工具变量宏
* * 估计 1、3、5、7 列 * * *
reg LWKLYWGE EDUC $YR
est sto ols1
reg LWKLYWGE EDUC $YR AGEQ AGEQSQ
est sto ols2
reg LWKLYWGE EDUC RACE MARRIED $YR $RG
est sto ols3
reg LWKLYWGE EDUC RACE MARRIED AGEQ AGEQSQ $YR $RG
est sto ols4
* * 估计 2、4、6、8 列 * * *
ivregress 2sls LWKLYWGE $YR (EDUC = $IV)
est sto iv1
ivregress 2sls LWKLYWGE $YR AGEQ AGEQSQ (EDUC = $IV)
est sto iv2
ivregress 2sls LWKLYWGE RACE MARRIED SMSA $YR $RG (EDUC = $IV)
est sto iv3
ivregress 2sls LWKLYWGE RACE MARRIED $YR $RG AGEQ AGEQSQ (EDUC = $IV)
est sto iv4
* * 估计结果表格输出到文件～angrist.rtf * *
esttab ols1 iv1 ols2 iv2 ols3 iv3 ols4 iv4 using angrist.rtf. ///
drop($YR $RG _cons) star(* .10 * * .05 * * * .01) b(%8.4f) se(%8.4f) ar2 ///
mtitle("OLS" "2SLS" "OLS" "2SLS" "OLS" "2SLS" "OLS" "2SLS") nogap replace
```

得到的估计结果见表 7.3：