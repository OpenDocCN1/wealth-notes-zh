# Python 机器学习与量化投资

采用 web 作为 pandas_datareader 模块的缩写是为了和老版本的 pandas 保持一致。

pandas_datareader 模块在作为独立模块以前，是 pandas 系统中子模块 io 的组成部分，原来的导入代码如下：

```
import pandas.io.data as web
```

同样，为了兼容和延续习惯，我们在这里还是用 web 作为模块的缩写，也有很多使用 pdr 作为 pandas_datareader 模块的缩写，这也是将 pandas、data、reader 三个单词首字母的缩式，也是一种常用的缩写习惯。

下面，我们分组对示例代码进行说明。

第1组代码如下：

```
jpy = web.DataReader('USDJPY','stooq')
jpy.to_csv('tmp/usdjpy2018.csv')
zr.pdf('1:1 jpy', jpy)
```

调用 pandas_datareader 模块的 DataReader 函数，读取 USDJPY（美元/日元交易对）的交易数据。

DataReader 的函数是一个一站式数据读取、产生的函数，其中的名词含义如下。

+   - USDJPY，外汇交易代码。
- stooq，数据源名称，这里表示 stooq.com 金融数据网站。

DataReader 函数功能很强，提供了起始时间、结束时间等参数。此外， pandas_datareader 模块还针对各种不同的金融数据网站，提供了独立的下级模块，这些源自于业的领域，有兴趣研究金融数据抓取的读者可以自行开发相关的源码。

第1组代码，对应的输出信息如下：

```
01 jpy
Open High Low Close
Date
```