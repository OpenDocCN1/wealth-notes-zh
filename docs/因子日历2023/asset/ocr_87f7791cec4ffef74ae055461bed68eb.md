# 趋势因子

以月度因子为例，在每月 t 最后一个交易日 d 计算每只股票在不同时间尺度 L 的移动平均价格作为趋势信号：

$MA_{jt,L} = \frac{P_{j,d-L+1}^{t} + P_{j,d-L+2}^{t} + \dots + P_{j,d-1}^{t} + P_{j,d}^{t}}{L}$

# 其中

$P_{j,d}^{t}$ 为股票 j 在第 t 月的每个交易日的收盘价，L是移动平均的窗口宽度；

由于不同股票的价格在量级上存在差异，为了使趋势信号在截面上具有可比性，将移动平均价格序列除以d日最新收盘价，做标准化处理：$M\tilde A_{jt,L} = \frac{MA_{jt,L}}{P_{jd}^{t}}$

不同时间尺度 $L_i$ ($L$=3、5、10、20、50、100、200、400、600、800以及1000个交易日)上的移动均线即为不同级别的趋势信号，每月末使用这些趋势信号和下一期的收益率进行截面回归，得到对应的因子收益率 $\beta_{i,t}$：$r_{j,t} = \beta_{0,t} + \sum_{i} \beta_{i,t} \tilde M A_{jt-1,L_i} + \varepsilon_{j,t}$

对各趋势信号，使用各自过去12个月的 $\beta_{i,t}$ 来计算下个月的预期因子收益率：$E_{t}[\beta_{i,t+1}] = \frac{1}{12} \sum_{m=1}^{12} \beta_{i,t+1-m}$

使用预期收益率和个股 j 在 t 期的最新均线指标取值计算出每支股票在 t+1 的收益率预测值即为趋势因子：$E[r_{i,t+1}] = \sum_{i} E_{t}[\beta_{i,t+1}] \tilde M A_{jt,L_i}$

# 说明

趋势因子综合考量了在不同时间点上，不同时间跨度的动量或者反转因子对股票未来收益的贡献。

〔参考文献〕
Han, Y., Zhou, G., Y. Zhu, 2016, A trend factor: any economic gains from using information over investment horizons? Journal of Financial Economics, 122(2), 352-375, https://zhuanlan.zhihu.com/p/51043407.