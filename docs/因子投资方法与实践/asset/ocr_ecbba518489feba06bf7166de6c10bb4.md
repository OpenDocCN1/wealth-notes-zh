的实证结果）。综合以上两点，短期历史收益率和F-Score叠加后与预期收益率的关系如表5.5所示。

# 表5.5 短期历史收益率以及F-Score叠加后和预期收益率的关系

|  | 短期历史收益 | F-Score | 二者叠加 |
|---|---|---|---|
| 过去输家 + 高 F-Score | 正相关 | 正相关 | 正相关 |
| 过去输家 + 低 F-Score | 正相关 | 负相关 | 模糊 |
| 过去赢家 + 高 F-Score | 负相关 | 正相关 | 模糊 |
| 过去赢家 + 低 F-Score | 负相关 | 负相关 | 负相关 |

由表5.5所示的关系可知，通过做多基本面好（F-Score高）的过去输家，并同时做空基本面差（F-Score低）的过去赢家便可以提升传统短期反转异象，获取更丰厚的超额收益。而上面这种处理方式，实际上和5.1.4节有异曲同工之妙，也是在寻找市场和基本面之间的预期差。其中，过去的输家叠加高F-Score的组合是被低估的股票，而过去赢家叠加低F-Score的组合是被高估的股票。投资者通过做多被低估的、做空被高估的获得超额收益。上述剥离了基本面信息的反转异象被称为基本面锚定反转（fundamental-anchored reversal，FAR）。

# 5.2.2 A股市场中的基本面锚定反转

下面针对A股进行基本面锚定反转的实证分析。实证中使用过去一个月的累计收益率为反转变量，使用5.1节介绍的F-Score为基本面变量，构建改进后的反转异象。为此，使用上述两个变量进行独立双重排序。具体来说，使用反转变量将股票分成5组（Loser、P2、P3、P4、Winner），使用F-Score将股票分成3组（Low、Middle、High），这样一共得到15个投资组合，如图5.9所示。

表5.6给出了以上15个组合在本书记证期内的月均收益率以及它们的t-值。为了考察结果的稳健性，表中同时汇报了等权重（Panel A）和市值加权（Panel B）的情况。受到Stein（2009）的启发，Zhu et al.（2019）提出通过做多Loser/High组合（基本面强的输家）同时做空Winner/Low组合（基本面差的赢家）就得到了基本面锚定反转。作为对比，通过做多Loser/Low组合（基本面差的输家）同时做空Winner/High组合（基本面强的赢家）得到未经基本面锚定的反转（fundamental-unanchored reversal，FUR）。表5.6中展示了FAR和FUR的月均收益率及其显著性。最后，表中同样列出了传统的反转（即Loser-Winner组合）的检验结果。

F-Score
Low (0 - 3)   Middle (4 - 6)   High (7 - 9)
过去一个月收益率
Loser   Loser / Low   Loser / Middle   Loser / High
P2      P2 / Low      P2 / Middle      P2 / High
P3      P3 / Low      P3 / Middle      P3 / High
P3      P4 / Low      P4 / Middle      P4 / High
Winner  Winner / Low  Winner / Middle  Winner / High