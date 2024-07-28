# 典型回答
ZSet（也称为Sorted Set）是Redis中的一种特殊的数据结构，它内部维护了一个有序的字典，这个字典的元素中既包括了一个成员（member），也包括了一个double类型的分值(score)。这个结构可以帮助用户实现记分类型的排行榜数据，比如游戏分数排行榜，网站流行度排行等。

**Redis中的ZSet在实现中，有多种结构，大类的话有两种，分别是ziplist(压缩列表)和skiplist(跳跃表)，但是这只是以前，在Redis 5.0中新增了一个listpack（紧凑列表）的数据结构，这种数据结构就是为了替代ziplist的，而在之后Redis 7.0的发布中，在Zset的实现中，已经彻底不在使用zipList了。**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/5378072/1683438825430-ef670c73-c5f0-4a91-9795-34a9dd5e1420.png#averageHue=%23fcf3ea&clientId=u60e751b5-08ff-4&from=paste&height=413&id=u3373c67f&originHeight=413&originWidth=1490&originalType=binary&ratio=1&rotation=0&showTitle=false&size=51914&status=done&style=none&taskId=ua6cdae1d-80d8-47a8-b4c7-a603be7f713&title=&width=1490)

当ZSet的元素数量比较少时，Redis会采用ZipList（ListPack）来存储ZSet的数据。ZipList（ListPack）是一种紧凑的列表结构，它通过连续存储元素来节约内存空间。当ZSet的元素数量增多时，Redis会自动将ZipList（ListPack）转换为SkipList，以保持元素的有序性和支持范围查询操作。

在这个过程中，Redis会遍历ZipList（ListPack）中的所有元素，按照元素的分数值依次将它们插入到SkipList中，这样就可以保持元素的有序性。

**在Redis的ZSET具体实现中，SkipList的这种实现，不仅用到了跳表，还会用到dict（字典）**

![image.png](https://cdn.nlark.com/yuque/0/2023/png/5378072/1683438896470-378a453b-191d-42e3-ac97-e26b3fd6daa9.png#averageHue=%23fcf7f2&clientId=u60e751b5-08ff-4&from=paste&height=639&id=u37ac8b25&originHeight=639&originWidth=1681&originalType=binary&ratio=1&rotation=0&showTitle=false&size=86197&status=done&style=none&taskId=u08c8f6d5-0985-4a89-ad49-8bfa37a6ecf&title=&width=1681)

其中，SkipList用来实现有序集合，其中每个元素按照其分值大小在跳表中进行排序。跳表的插入、删除和查找操作的时间复杂度都是 O(log n)，可以保证较好的性能。

dict用来实现元素到分值的映射关系，其中元素作为键，分值作为值。哈希表的插入、删除和查找操作的时间复杂度都是 O(1)，具有非常高的性能。

![image.png](https://cdn.nlark.com/yuque/0/2023/png/5378072/1683440190031-b12ca284-ef28-4862-a995-76081de09ad6.png#averageHue=%23f1ebe2&clientId=u60e751b5-08ff-4&from=paste&id=u3071278a&originHeight=741&originWidth=1031&originalType=url&ratio=1&rotation=0&showTitle=false&size=99015&status=done&style=none&taskId=u69220225-118b-4e84-9ba7-07df6c446a2&title=)

# 扩展知识

## 何时转换


ZipList（ListPack）和SkipList之间是什么时候进行转换的呢？

<br />在 Redis 中，ZSET在特定条件下会使用 ziplist作为其内部表示。这通常发生在有序集合较小的时候，具体条件如下：

1. 元素数量少：集合中的元素数量必须小于某个阈值（zset-max-ziplist-entries）。
2. 元素大小小：集合中的每个元素（包括值和分数）的大小必须小于指定的最大值（zset-max-ziplist-value）。

默认情况下，zset-max-ziplist-entries是128，zset-max-ziplist-value是64。

**总的来说就是，当元素数量少于128，每个元素的长度都小于64字节的时候，使用ZipList（ListPack），否则，使用SkipList！**

## 跳表

跳表也是一个有序链表，如下面这个数据结构：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/5378072/1671868102611-a17cd7ab-cc42-445a-b7cf-001a14ab979b.png#averageHue=%23f9fcfc&clientId=u2eb60dfb-0b47-4&from=paste&height=43&id=u8620f2bd&originHeight=43&originWidth=1322&originalType=binary&ratio=1&rotation=0&showTitle=false&size=34556&status=done&style=none&taskId=u8b5dab90-a333-459b-b5da-792beb031e6&title=&width=1322)

在这个链表中，我们想要查找一个数，需要从头结点开始向后依次遍历和匹配，直到查到为止，这个过程是比较耗费时间的，他的时间复杂度是0（n）。

当我们想要向这个链表中插入一个数的时候，过程和查找类似，先需要从头开始遍历找到合适的为止，然后再插入，他的时间复杂度也是 O(n)。

那么，怎么能提升遍历速度呢，有一个办法，那就是我们对链表进行改造，先对链表中每两个节点建立第一级索引，如下图所示：

![image.png](https://cdn.nlark.com/yuque/0/2022/png/5378072/1671868119609-f901bb87-1a52-4aee-b5ac-bbc9ebc04e80.png#averageHue=%23f9faf9&clientId=u2eb60dfb-0b47-4&from=paste&height=82&id=u0c549c47&originHeight=82&originWidth=1321&originalType=binary&ratio=1&rotation=0&showTitle=false&size=35604&status=done&style=none&taskId=u5a7dabc6-9fdc-4f03-9fd3-120ef0987c9&title=&width=1321)

有了我们创建的这个索引之后，我们查询元素12，我们先从一级索引6 -> 9 -> 17 -> 26中查找，发现12介于9和17之间，然后，转移到下一层进行搜索，即9 -> 12 -> 17，即可找到12这个节点了。

可以看到，同样是查找12，原来的链表需要遍历5个元素(3、6、7、9、12)，建立了一层索引之后，只需要遍历4个元素即可（6、9、17、12）。

像上面这种带多级索引的链表，就是跳表。
