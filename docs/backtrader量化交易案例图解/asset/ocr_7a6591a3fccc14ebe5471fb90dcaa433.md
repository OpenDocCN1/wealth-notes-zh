```
#
print('\t 读取股票池、指数池数据')
#tq.pools_get4flst(qx,rbas0,baslst,tim0str,tim9str,fgInx=True)
tq.pools_get4flst(qx,rsyb0,syblst,tim0str,tim9str,fgInx=False)

#------------
print('\n#2-2,设置 BT 参数, anzMod 为分析模式, 默认0')
tq.bt_set(qx,anzMod=1)
#
#
#--------
print('\n#3,调用 BT 回测入口程序, 执行 opt 参数优化函数')
#原版 optstrategy 策略优化函数 API 接口
##cerebro.optstrategy(MyStrategy, myparam1=range(10, 20))
#
print('\n#3-1,单参数自动寻优测试')
#d10=list(range(5, 50,5))
#sgnlst=['period']
#x100=tq.bt_opt(qx,ma,[d10],sgnlst)
#xxx
#
print('\n#3-2,多参数自动寻优测试')
d10=list(range(5, 35,5))
d20=list(range(10, 190,20))
sgnlst=['nfast','nslow']
vlst=[d10,d20]
x100=tq.bt_opt(qx,ma_cross,vlst,sgnlst)

#------------
print('\n@x.end#9, 没有plot 绘图, analyzer 分析部分')
#qx.plot(figsize=(1500,500))

print('\nzok')
```