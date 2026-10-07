# 基于排序的动量

R_{i,d} 为股票 i 在 d 天的日收益率，y(R_{i,d}) 为横截面上 R_{i,d} 的升序排名；

计算每日每只股票收益率排名的标准化得分(Wright, 2000):

```
rank_{i,d} = ( y(R_{i,d}) - (N_d + 1)/2 ) / sqrt( (N_d + 1)(N_d - 1) / 12 )
```

计算每月每只股票收益率排名的标准化得分均值：

```
rank_{i,D} = 1/D sum_{d=1}^{D_j} rank_{i,d}
```

计算因子考察期（T0-N-M到T0-M区间内）内股票收益率排名的标准化得分均值：

```
rank_{i,d}(N, M) = 1/N sum_{j=t-N-M}^{t-M} (rank_{i,D_j})
```

# 说明

相比动量的传统构造方式（只考虑区间头尾的价格），RANK动量考虑到了区间内每日的收益率情况，并且采用rank排名来替代收益率绝对大小，可以降低异常股价波动的影响，是一种更加稳健的构建方式。

[参考文献]
① 朱剑涛, 刘舜涵, 2019, 存在于全市场范畴内的稳健动量效应, 东方证券.
② Chen T Y, Chou P H, Ko K C, et al. Rank, Sign, and Momentum [J].
③ Wright, J. H. 2000, Alternative variance-ratio tests using ranks and signs. Journal of Business and Economic Statistics 18, 1-9.