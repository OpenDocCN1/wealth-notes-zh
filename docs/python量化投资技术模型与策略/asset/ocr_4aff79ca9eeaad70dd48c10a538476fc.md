# 11.4.2 数据库

存储数据有很多种方式。有人比较喜欢以文件的形式存储，比如，csv文件或hdf5文件。但通常情况下，笔者还是比较喜欢建立一个数据库，以便于进行数据的管理和分享。而且，数据库本身也能完成很多数据计算和分析工作。

现今的数据库主要分为两种：一种是关系型数据库，也就是使用SQL语句进行操作的数据库，常见的有MySQL、PostgreSQL、SQL Sever等；一种是非关系型（NoSQL）数据库，比如MongoDB。这两种数据库各有其优势，这里主要还是使用传统的SQL数据库。本书中以PostgreSQL为例。SQL基础知识的讲解不在本书的范围之内，大家可以自行查找相关的教程进行学习。这里假设读者已经具有SQL的相关基础。

Pandas利用SQLAlchemy包，提供了简单的数据库接口。比如，我们想把一个dataframe存储到数据库里面，可以使用如下代码：

```
from sqlalchemy import create_engine
engine =
create_engine('postgresql://postgres@localhost:5432/postgres')
df.to_sql('stock_all_info',engine,if_exists='replace',index=True)
```