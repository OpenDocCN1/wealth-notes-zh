```
def get_data_amount_count(df,top):
    count = []
    for c in df.columns:
        count.append([c,len(df[c].dropna())])
    count = sorted(count,key = lambda x:x[1], reverse = True)
    valid_ids = [] #获得数据量足够的股票代码
    for i in range(0,top):
        valid_ids.append(count[i][0])
    return valid_ids,count[top][1]
valid_ids,amount_of_data = get_data_amount_count(sz380close,top)

sz380close = sz380close[valid_ids].iloc[len(sz380)-amount_of_data:,:].fillna
    (method = 'ffill')
sz380return = sz380close.pct_change().dropna()
sz380mc = sz380mc[valid_ids].iloc[len(sz380)-amount_of_data:,:].fillna(method =
    'ffill').dropna()
sz380pe = sz380pe[valid_ids].iloc[len(sz380)-amount_of_data:,:].fillna(method =
    'ffill').dropna()
```

下面就来正式构建因子。我们先构建SMB因子。SMB因子是由做多市值排名相对较小，而做空市值排名相对较大的股票获得的因子。针对数据内的每一个交易日，我们根据市值来进行选股，代码如下：

```
sz380mc_dict = sz380mc.to_dict(orient = 'index')
factor_track = []
for date, data in sz380mc_dict.items():
    #我们分别取市值排名前10%和后10%
    data = list(data.items())
    data = sorted(data,key = lambda x:x[1])
    small_cap = data[:int(0.1*len(data))]
    large_cap = data[int(0.9*len(data)):] 
    #将每一个交易日的SMB因子构建出来
    #我们先将投资轨迹建立起来
    factor_track.append([date,[x[0] for x in small_cap],[x[0] for x in large_cap]])
#再依据投资轨迹建立相应的因子收益轨迹
factor = []
for track in factor_track:
```