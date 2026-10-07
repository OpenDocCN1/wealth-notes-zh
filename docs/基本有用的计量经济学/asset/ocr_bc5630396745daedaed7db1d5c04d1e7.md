```
r(N)      =  74
r(sum_w)  =  74
r(mean)   =  21.2972972972973
r(Var)    =  33.47204738985561
r(sd)     =  5.785503209735141
r(min)    =  12
r(max)    =  41
r(sum)    =  1576
```

下面是做了一个简单的线性回归模型后 e() 所保存的结果。

```
. qui reg mpg weight price, vce(robust)
. eret list
scalars:
```

```
e(N)      =  74
e(df_m)   =  2
e(df_r)   =  71
e(F)      =  51.14373982809451
e(r2)     =  .6531446579233133
e(rmse)   =  3.454996314099513
e(mss)    =  1595.932492798133
e(rss)    =  847.5269666613266
e(r2_a)   =  .6433740849070686
e(ll)     =  -195.2169813478502
e(ll_0)   =  -234.3943376482347
e(rank)   =  3
```

macros:

```
e(cmdline)   : "regress mpg weight price, vce(robust)"
e(title)      : "Linear regression"
e(marginsok)  : "XB default"
e(vce)        : "robust"
e(depvar)     : "mpg"
e(cmd)        : "regress"
e(properties) : "b V"
e(predict)    : "regres_p"
e(model)      : "ols"
e(estat_cmd)  : "regress_estat"
e(vcetype)    : "Robust"
```