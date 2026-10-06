# 从零开始学 Python 大数据与量化交易

业分类，如 A01（农业）、A02（林业）、B06（煤炭开采和洗选业）、B07（石油和天然气开采业）、C36（汽车制造业）等。

返回值是一个哑变量，如果某股票属于某行业，则返回 1；否则，返回 0。

> 提醒 哑变量，又称虚设变量、名义变量，是量化了的质变量，通常取值为 0 或 1。

# 4. 概念因子

概念因子，即概念板块代码，如 GN028（智能电网）、GN030（物联网）、GN092（高端装备制造）、GN181（一带一路）等。

返回值是一个哑变量，如果某股票属于某个概念，则返回 1；否则，返回 0。

# 5. 指数因子

指数因子，即指数代码，如 000001.XSHG（上证指数）、000002.XSHG（A 股指数）、000003.XSHG（B 股指数）、000006.XSHG（地产指数）等。

返回值是一个哑变量，如果某股票属于某个指数，则返回 1；否则，返回 0。

# 6. 资金流因子

资金流因子，即 get_money_flow() 函数查询的数据。可以使用的字段包括 change_pct（涨跌幅）、net_amount_main（主力净额）、net_pct_main（主力净占比）、net_amount_xl（超大单净额）、net_pct_xl（超大单净占比）、net_amount_l（大单净额）、net_pct_l（大单净占比）、net_amount_m（中单净额）、net_pct_m（中单净占比）、net_amount_s（小单净额）、net_pct_s（小单净占比）。

# 15.2.3 calc 的参数及返回值

在 calc 中，可以通过 data 参数获取 max_window 和 dependencies 定义的数据。参数 data 是一个 dict 对象，其 key 属性是 dependencies 中的因子名称，value 属性是 pandas.DataFrame。

DataFrame 的 column 是股票代码，可以选择沪深 300 作为股票池。

DataFrame 的 index 是一个时间序列，结束时间是当前时间，长度是 max_window。

calc 保证返回 pandas.Series，其中 index 属性是股票代码，value 属性是因子值。

# 15.3 因子分析的结果

前面讲解的因子分析的基础知识及实现代码，下面利用聚宽 JoinQuant 量化交易平台新建因子，并查看因子分析的结果。