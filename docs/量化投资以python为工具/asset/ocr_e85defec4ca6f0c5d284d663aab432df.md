# 26.2 配对交易的思想

配对交易投资者甚少关注单只股票价格的绝对高低，也不受股票市场的走势方向左右；其投资标的在于股票对的价差，挑选出价格差异长期稳定，并且可能有机会被高估或被低估的股票对，获取价差扩张再收敛而产生的波动收益。多空配对的投资组合减低了整体股市未来走势不确定的市场风险，其主要风险来自于配对股票未来发展的不确定性。因此，多空部位同时建仓的配对交易策略被视为一种市场中性策略。

配对交易的思想最早起源于 20 世纪 20 年代，华尔街交易员 Jesse Lauriston Livermore 在投资实战中采用的姐妹对（Sister Stocks）交易策略。他发现两只同行业内业务相似的股票，其价格波动趋势有某种规律，即两者的价差会构成一种均衡关系。若在两股票价格偏离较大时建仓，价差回复时平仓，便会赚取价差由发散至收敛带来的收益。随着数理金融学和计算机技术的不断发展，到了 20 世纪 80 年代，配对交易策略在华尔街得到巨大关注。1985 年，华尔街投资银行摩根士丹利（Morgan Stanley）的天体物理学家努齐奥·塔塔里亚（Nunzio Tartaglia）组建了一个由著名物理学家、计算机科学家和数学家构成的数量化分析团队，该团队以数学模型计算股票买卖组合，并开发计算机自动交易程序，在当时华尔街投资实践中大获成功。该程序运用的买卖组合策略即是配对交易策略，与传统的主观技术分析不同，摩根士丹利的交易程序对于股票对的选择、交易参数的设定与交易规则的制定，都采取量化分析的手段并由计算机程序自动完成，自此以后，这种量化投资策略逐渐广为人知并流传开来。时至今日，配对交易已经衍生出众多模型和交易规则，并为各种避险基金和股票投资者使用。

关于配对交易的学术研究也不胜枚举，Gatev, Goetzmann 和 Rouwenhorst（1999）$^{1}$ 对 1962 年 6 月到 1997 年 12 月的美国股市进行配对交易研究，他们采用最小距离法寻找历史价格最近的 20 组股票对，得到每一配对的标准价差（$\hat{\rho}$）。进场交易的时机为标准化价差 $\hat{\rho}$ 超过两倍标准差（$\sigma_{\hat{\rho}}$），即 $\hat{\rho}>2\sigma_{\hat{\rho}}$ 之时；运用买低价股卖出高价股的策略，年化报酬率可达到 11% 以上。Hong and Sussel（2003）$^{2}$ 选择亚洲 64 只股票与其在美国挂牌的 ADRs，以相对价值为交易指标，在 1991 年第一季到 2000 年最后一季中，分别以 3、6、12 个月作为交易期间，结果显示，随着持有期间由 3 个月增加到 12 个月，年化报酬亦从 8.5% 上升到 33.8%。Vidyamurthy（2004）$^{3}$ 把协整理论运用到配对交易的股票对选择中，并将平稳性作为配对好坏的判断标准。从股票基本面或历史资料挑选出潜在具有协整关系之配对股票，并以协整系数及均衡值两种指标来建构两种股票价格之线性关系。而 Agarwal, Madhogaria 等（2004）$^{4}$ 在跨产业股票池中，先用各股价之平均数与标准差将股

¹Gatev, Evan G., Goetzmannn, William and Rouwenhorst, K., (1999), *Pairs Trading: Performance of a Relative Value Arbitrage Rule*, No 7032, NBER Working Papers, National Bureau of Economic Research, Inc.

²Hong, Gwanghoon, and Raul Sussel, “Pairs-trading in the Asian ADR market.” University of Houston, Unpublished Manuscript. (2003).

³Vidyamurthy, Ganapathy. *Pairs Trading: quantitative methods and analysis*. Vol. 217. John Wiley & Sons, 2004.

⁴Agarwal, Nikesh, Vikash Madhogaria, and Supreema Narayanan. *Simulated Trading-An Analysis of Pairs Trading*.