```
xret, xpos, xtran, gross_lev = xpyf.get_pf_items()
#print(xret);print('xx',type(xret))
#
#xret.to_csv('tmp/x_ret.csv',index=True,header=None,encoding='utf8')
#gross_lev.to_csv('tmp/x_gros.csv',index=True,header=None,encoding='utf8')
#
#xpos.to_csv('tmp/x_pos.csv',index=True,encoding='utf8')
#xtran.to_csv('tmp/x_tran.csv',index=True,encoding='utf8')

#
#xret=to_series(to_utc(xret))
xret=to_utc(xret)
xpos=to_utc(xpos)
xtran=to_utc(xtran)
#print('\n@xret',xret);#print('xx',type(xret))
#print('\n@xpos',xpos);#print('xx',type(xpos))
#print('\n@xtran',xtran);#print('xx',type(xtran))
#xxx
#

#----
#qx.tim0str,qx.tim9str tim0str,tim9str='2015-01-01','2018-12-31'
#xtim0=dt.datetime(2015,1,1)
pf.create_full_tear_sheet(xret
    ,positions=xpos
    ,transactions=xtran
    ,benchmark_rets=xret
    #,bayesian=True
    #,gross_lev=gross_lev
    #,live_start_date=xtim0 # This date is sample specific
    #,round_trips=True
)
#

print('\n#9,绘制BT量化分析图形')
cerebro.plot()
```