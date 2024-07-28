Redis支持的数据类型主要有五种，它们分别是：
### **String（字符串）**

- 字符串是 Redis 中最简单和最常用的数据类型。可以用来存储如字符串、整数、浮点数、图片（图片的base64编码或图片的路径）、序列化后的对象等。
- 每个键（key）对应一个值（value），一个键最大能存储512MB的数据。
- 应用场景包括缓存、配置信息、计数器等。
```
SET key "value"
GET key
```
### **Hash（哈希）**

- Redis Hash是一个String类型的field和value的映射表，类似于Java中的Map<String, Object>。
- Hash特别适合用于存储对象，如用户信息、商品详情等。
- 每个Hash可以存储2^32 - 1个键值对。
```
HSET user:1000 name "John"
HGET user:1000 name
```
### **List（列表）**

- 列表是一个有序的字符串集合。
- 列表可以从两端压入或弹出元素，支持在列表的头部或尾部添加元素。
- 列表最多可存储2^32 - 1个元素。
- 应用场景包括社交网络的时间线、任务队列等。
```
LPUSH mylist "world"
LPUSH mylist "hello"
LRANGE mylist 0 -1
```
### **Set（集合）**

- Set是一个无序的字符串集合，不允许重复元素。集合适用于去重和集合运算（如交集、并集、差集）。
- Set的添加、删除、查找操作的复杂度都是O(1)。
- 应用场景包括用户标签管理、黑名单管理等。
```
SADD myset "hello"
SADD myset "world"
SMEMBERS myset
```
### **Zset（有序集合）**

- Zset和Set一样也是String类型元素的集合，且不允许重复的成员。
- 有序集合类似于集合，但每个元素都会关联一个double类型的分数（score），redis正是通过分数来为集合中的成员进行从小到大的排序。
- 应用场景包括评分排名、热度排名等。

此外，Redis还提供了一些特殊的数据类型，如HyperLogLog（基数统计）、Bitmap（位图）、Geospatial（地理位置）等
