[![18、mysql分页导致查询数据重复案例.mp4 (13.26MB)](https://gw.alipayobjects.com/mdn/prod_resou/afts/img/A*NNs6TKOR3isAAAAAAAAAAABkARQnAQ)](https://www.yuque.com/docs/176645994?_lake_card=%7B%22status%22%3A%22done%22%2C%22name%22%3A%2218%E3%80%81mysql%E5%88%86%E9%A1%B5%E5%AF%BC%E8%87%B4%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE%E9%87%8D%E5%A4%8D%E6%A1%88%E4%BE%8B.mp4%22%2C%22size%22%3A13902510%2C%22taskId%22%3A%22uf5579291-bfde-4e69-ad5e-93ab1e45657%22%2C%22taskType%22%3A%22upload%22%2C%22url%22%3Anull%2C%22cover%22%3Anull%2C%22videoId%22%3A%22inputs%2Fprod%2Fyuque%2F2024%2F29413969%2Fmp4%2F1719845637224-32da46e8-ba5e-4880-aa28-b821be136537.mp4%22%2C%22download%22%3Afalse%2C%22__spacing%22%3A%22both%22%2C%22id%22%3A%22osb4X%22%2C%22margin%22%3A%7B%22top%22%3Atrue%2C%22bottom%22%3Atrue%7D%2C%22card%22%3A%22video%22%7D#osb4X)这个问题挺经典的，也是比较经典的线上案例排查。估计小伙伴们也或多或少遇到过一些。<br />本排查案例是通用的！大家消化完成，直接套到自己项目即可！<br />本题配备了视频助于理解。
# 问题现象
在管理系统经典写分页查询接口的时候，order by和limit混用的时候，出现了排序的混乱情况，在进行第N页查询时，出现与第一前面页码的数据一样的记录。再直白一点就是，如:limit(0,20)表示查询第一页的20条数据，limit(20,20)表示查询第二页的数据。业务上我们通常也会在分页的时候加上排序 order by;<br />但是当limit和order by一起使用的时候，有可能会出现第N页的数据，竟然和前面页码的数据有重复。
# 举个例子
例如：<br />SELECT a,b FROM table WHERE c=1 ORDER BY d desc LIMIT 0,20<br />这个 SQL 在查询的时候，很有可能和LIMIT 20,20查出相同的某条数据。为了解决这个问题，我们在ORDER BY后面加上了ID（唯一索引页可以）排序来进行规避。d 列可能是重复的值。

如下：<br />SELECT a,b FROM table WHERE c=1 ORDER BY d desc，id desc LIMIT 0,20

理论上，MySQL的排序默认情况下是以主键ID作为排序条件的，也就是说，如果在条件d相等的情况下，主键id会作为默认的排序条件，不需要我们多此一举加ID asc。但是事实就是，MySQL在order by和limit同时使用的情况下，出现了排序的混乱情况。
## 分析
在MySQL 5.6的版本上，优化器在遇到order by+limit语句的时候，做了一个优化，使用了priority queue。<br />使用 priority queue 的目的，就是在不能使用索引有序性的时候，如果要排序，并且使用了limit n，那么只需要在排序的过程中，保留n条记录即可，这样虽然不能解决所有记录都需要排序的开销，但是只需要 sort buffer 少量的内存就可以完成排序。<br />之所以MySQL 5.6出现了第二页数据重复的问题，是因为 priority queue 使用了堆排序的排序方法，而堆排序是一个不稳定的排序方法，也就是相同的值可能排序出来的结果和读出来的数据顺序不一致。<br />MySQL 5.5 没有这个优化，所以也就不会出现这个问题。<br />也就是说，MySQL 5.5是不存在本文提到的问题的，5.6版本之后才出现了这种情况。

正常执行的相关顺序：
> (1)     SELECT 
> (2)     DISTINCT <select_list>
> (3)     FROM <left_table>
> (4)     <join_type> JOIN <right_table>
> (5)     ON <join_condition>
> (6)     WHERE <where_condition>
> (7)     GROUP BY <group_by_list>
> (8)     HAVING <having_condition>
> (9)     ORDER BY <order_by_condition>
> (10)    LIMIT <limit_number>
> 执行顺序依次为 form… where… select… order by… limit…


由于上述priority queue的原因，在完成select之后，所有记录是以堆排序的方法排列的，在进行order by时，仅把d值大的往前移动。但由于limit的因素，排序过程中只需要保留到20条记录即可，d并不具备索引有序性，所以当第二页数据要展示时，mysql见到哪一条就拿哪一条，因此，当排序值相同的时候，第一次排序是随意排的，第二次再执行该sql的时候，其结果应该和第一次结果一样。
## 解决方法
### 尽量使用不重复的值进行排序
如果在字段添加上索引，就直接按照索引的有序性进行读取并分页（这个字段如果有重复值分页会有可能出现重复）。可以最后加上ID排序，也不会影响业务。保证尽量不重复即可。

