当Redis的内存用完时，会根据配置的内存回收策略采取不同的措施。可以在内存达到限制时决定如何处理新的写请求。
### 内存回收策略

1. **noeviction**：不删除任何键，当内存不足时返回错误。这是默认策略。
2. **allkeys-lru**：使用最近最少使用（LRU）算法回收所有键。
3. **volatile-lru**：使用最近最少使用（LRU）算法回收设置了过期时间的键。
4. **allkeys-random**：随机回收所有键。
5. **volatile-random**：随机回收设置了过期时间的键。
6. **volatile-ttl**：回收那些剩余生存时间（TTL）最短的键。
7. **volatile-lfu**：使用最近最少使用（LFU）算法回收设置了过期时间的键。
8. **allkeys-lfu**：使用最近最少使用（LFU）算法回收所有键。
### 配置内存回收策略
redis.conf文件中配置内存回收策略，例如：
```
maxmemory 100mb
maxmemory-policy allkeys-lru
```
通过命令行参数设置：
```
redis-server --maxmemory 100mb --maxmemory-policy allkeys-lru
```
### 内存用完时的行为
根据不同的内存回收策略，当Redis的内存用完时，会发生以下行为：
#### 1. noeviction
当内存达到限制时，Redis将不再接受任何写请求，并返回错误。例如，客户端尝试设置新键时，会收到类似以下的错误信息：
```
(error) OOM command not allowed when used memory > 'maxmemory'.
```
#### 2. allkeys-lru 和 volatile-lru
Redis将根据LRU算法选择最近最少使用的键进行删除，以腾出空间存储新的数据。allkeys-lru会在所有键中选择，volatile-lru只会在设置了过期时间的键中选择。
#### 3. allkeys-random 和 volatile-random
Redis会随机选择一些键进行删除，以腾出空间。allkeys-random会在所有键中选择，volatile-random只会在设置了过期时间的键中选择。
#### 4. volatile-ttl
Redis将选择那些剩余生存时间（TTL）最短的键进行删除。
#### 5. volatile-lfu 和 allkeys-lfu
Redis将根据LFU算法选择最近最少使用的键进行删除。volatile-lfu只会在设置了过期时间的键中选择，allkeys-lfu会在所有键中选择。
