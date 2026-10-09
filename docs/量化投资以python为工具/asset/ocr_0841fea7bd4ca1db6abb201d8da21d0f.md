```
In [6]: df
Out[6]:
																												A		B		C		D
2016-01-01		-0.924813		1.011836		-0.312846		0.773170
2016-01-02		0.462387		-0.747073		1.064430		0.598022
2016-01-03		0.318700		-0.357701		-1.503945		-0.417211
2016-01-04		-0.185249		0.398682		1.541127		-0.968151
2016-01-05		1.161603		-0.634552		0.125405		-1.496913
2016-01-06		-0.796077		0.108933		0.950862		0.452635
```

除了手动创建数据之外，还可以从其他地方读取数据来创建 DataFrame 对象。

+   - read_table() 函数

对于多数常见的存储数据的文件，比如 .txt 文件或是 .csv 文件，都可以用 read_table() 函数来处理，该函数的调用形式为：

```
pd.read_table('data_file', sep='\t', header=None, names=None)
```

上面代码中的参数是几个比较常见的参数，其中 'data_file' 指定了需要读入的文档(包含路径); sep='\t' 表示字段之间的分隔符为 Tab 分隔符 (此为默认取值); header 是指用作列名 (变量名) 的行数，比如 header=0 (默认取值) 指的是第 0 行的数据用来做列名，header=None 是指没有列名; names 可以指定各列变量名，如果 header=0 则说明将第 0 行作为列名，此时可以将 names 取值为 None。read_table() 函数还有很多参数，有兴趣的读者可以用 pd.pandas( ) 来查看该函数的所有参数及用法。

+   - 读取 csv 文件数据

Pandas 库中还有一个专门处理 csv 文件的函数 read_csv()，该函数与 read_table() 的参数十分类似。假设我们要读取 test.csv 中的数据，对应的代码如下：

```
In [1]: df = pd_read_csv('filepath/test.csv', header=None, sep=',' )
```

其中，filepath 是 test.csv 文件所在的路径，比如在 D 盘的 data 文件夹里，则 filepath 为 D:/data; header=None 表示没有列表面，sep=',' 表示字段之间的分隔符为逗号 (默认值)。

+   - 读取 MySQL 数据

要读取 MySQL 中的数据，首先要安装 Pandas 支持的 MySQL 驱动，http://docs.sqlalchemy.org/en/latest/dialects/mysql.html列出了所有支持的 MySQL 驱动。下面以 MySQLdb 包为例来说明一下 Pandas 读取 MySQL 数据的方法。假设数据库 stock 安装在本地，用户名 root，密码为 pwd123，要读取 stock 数据库中的数据，对应的代码如下：

```
import pandas as pd
import MySQLdb
mysql_cn = MySQLdb.connect(host='localhost', port=3306,user='root',passwd='pwd123',
```