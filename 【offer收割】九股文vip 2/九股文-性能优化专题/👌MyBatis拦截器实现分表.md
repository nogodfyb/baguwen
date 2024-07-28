[![冷热数据分离mybatis拦截器分表.mp4 (15.8MB)](https://gw.alipayobjects.com/mdn/prod_resou/afts/img/A*NNs6TKOR3isAAAAAAAAAAABkARQnAQ)](https://www.yuque.com/docs/176645991?_lake_card=%7B%22status%22%3A%22done%22%2C%22name%22%3A%22%E5%86%B7%E7%83%AD%E6%95%B0%E6%8D%AE%E5%88%86%E7%A6%BBmybatis%E6%8B%A6%E6%88%AA%E5%99%A8%E5%88%86%E8%A1%A8.mp4%22%2C%22size%22%3A16562776%2C%22taskId%22%3A%22ufbb82777-9836-421e-aaf8-a77013ad9a0%22%2C%22taskType%22%3A%22upload%22%2C%22url%22%3Anull%2C%22cover%22%3Anull%2C%22videoId%22%3A%22inputs%2Fprod%2Fyuque%2F2024%2F29413969%2Fmp4%2F1720371045533-07267bcf-9807-42fc-bd7c-0877b811efd4.mp4%22%2C%22download%22%3Afalse%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22xP4rk%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22video%22%7D#xP4rk)# 背景
所负责的业务中有一个 xx 系统，其中的 xx 核心表增长速度非常快，每日增长 200W 的数据量，业务特性主要是查询最近一周的数据，那么随着数据量越来越多，查询性能疯狂下降。这是典型的冷热数据场景，于是要开始进行冷热分离的操作。<br />常见的两种方案，一种是删除/归档旧数据，一种是数据分表。
# <br />方案选择
## 归档/删除旧数据
这种方式最常见的发难就是将冷数据移动到归档表或者直接进行删除，从而减少表的大小。<br />实现思路非常简单，一般写一个定时任务不断的跑就可以了。<br />缺点：<br />1、数据删除会影响数据库性能，引发慢sql，多张表并行删除，数据库压力会更大。<br />2、频繁删除数据，会产生数据库碎片，影响数据库性能，引发慢SQL。<br />有一定的风险，选择另一种分表的方案。
## 分表
业务特性是查最近的一周的数据，按日期的水平拆分是合理的。每周生成一张新表，同时此表只放本周的数据，这样表内数据量得到了控制，不会很大。
### 分表方案选型
分别常见的主要考虑2种分表方案：Sharding-JDBC、利用Mybatis自带的拦截器特性。<br />经过对比后，决定采用Mybatis拦截器来实现分表。<br />1、JAVA生态中很常用的分表框架是Sharding-JDBC，虽然功能强大，但需要一定的接入成本，并且很多功能暂时用不上。<br />2、系统本身已经在使用Mybatis了，只需要添加一个mybaits拦截器，把SQL表名替换为新的周期表就可以了，没有接入新框架的成本，开发成本也不高。
### 分表具体实现思路
1、通过拦截器获取表名<br />String tableName = TemplateMatchService.matchTableName(boundSql.getSql().trim());<br />2、根据数据日期进行计算后缀<br />3、替换原有 sql，最终执行。<br /> String rebuildSql = boundSql.getSql().replace(shardingProperty.getTableName(), shardingTable);<br /> metaStatementHandler.setValue(ORIGIN_BOUND_SQL, rebuildSql);

