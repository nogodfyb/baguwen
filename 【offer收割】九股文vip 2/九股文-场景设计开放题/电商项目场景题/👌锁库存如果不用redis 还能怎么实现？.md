锁库存是电商系统中常见的需求，特别是在高并发场景下，需要确保库存的准确性和一致性。记住 Redis 是一种常见的解决方案，如果面试官非问这种，就答下面这些方案。
### 1. 数据库乐观锁
乐观锁是一种不使用数据库锁机制的锁定方式。它假设冲突很少发生，因此在更新数据时进行冲突检测。
#### 实现方法：

1. **添加版本号**： 在库存表中添加一个`version`字段，用于记录数据的版本。
2. **更新时检查版本号**： 在更新库存时，通过`version`字段来确保数据没有被其他事务修改。
#### 示例：
```
UPDATE inventory
SET stock = stock - 1, version = version + 1
WHERE product_id = ? AND version = ?;
```
如果更新成功，说明库存锁定成功；否则，说明有并发修改，需要重试。
### 2. 数据库悲观锁（不推荐）
悲观锁是一种锁定机制，确保在事务完成之前，其他事务无法访问被锁定的数据。
#### 实现方法：

1. **使用**`**SELECT ... FOR UPDATE**`： 在事务中使用`SELECT ... FOR UPDATE`语句锁定行，直到事务提交或回滚。
#### 示例 SQL：
```
BEGIN;

SELECT stock
FROM inventory
WHERE product_id = ?
FOR UPDATE;

-- 检查库存并更新
UPDATE inventory
SET stock = stock - 1
WHERE product_id = ?;

COMMIT;
```
悲观锁适用于冲突较多的场景，但可能会导致数据库锁争用，影响性能。
### 3. 分布式锁
分布式锁用于在分布式系统中确保资源的独占访问。可以使用 Zookeeper、Etcd 等工具实现分布式锁。
#### 实现方法：

1. **使用 Zookeeper**： 利用 Zookeeper 的临时节点和顺序节点特性，实现分布式锁。
#### 示例使用 Curator 框架：
```
CuratorFramework client = CuratorFrameworkFactory.newClient("zkServer:2181", new ExponentialBackoffRetry(1000, 3));
client.start();

InterProcessMutex lock = new InterProcessMutex(client, "/inventory_lock");
try {
    if (lock.acquire(10, TimeUnit.SECONDS)) {
        try {
            // 处理库存
        } finally {
            lock.release();
        }
    }
} catch (Exception e) {
    e.printStackTrace();
} finally {
    client.close();
}
```
### 4. 基于消息队列的异步处理（可用）
通过消息队列，将库存操作异步化，避免并发冲突。
#### 实现方法：

1. **消息队列**： 使用 Kafka、RabbitMQ 等消息队列，将库存操作请求发送到队列中。
2. **消费者处理**： 消费者从队列中读取消息，顺序处理库存操作。
#### 示例：
```
// 生产者
rabbitTemplate.convertAndSend("inventoryQueue", new InventoryMessage(productId, quantity));

// 消费者
@RabbitListener(queues = "inventoryQueue")
public void handleInventory(InventoryMessage message) {
    // 处理库存
}
```
### 5. 数据库事务（不要用不要用）
通过数据库事务，确保库存操作的原子性和一致性。
#### 实现方法：

1. **事务管理**： 在业务逻辑中使用数据库事务，确保库存操作的原子性。
#### 示例代码（使用 Spring 事务管理）：
```
@Transactional
public void updateInventory(Long productId, int quantity) {
    // 检查库存
    int stock = inventoryMapper.getStock(productId);
    if (stock >= quantity) {
        // 更新库存
        inventoryMapper.updateStock(productId, stock - quantity);
    } else {
        throw new InsufficientStockException();
    }
}
```
### 总结
锁库存的实现方式有很多种，选择合适的方案需要根据具体的业务需求和技术栈来决定。乐观锁和悲观锁适用于单体应用和简单的分布式场景；分布式锁适用于复杂的分布式系统；基于消息队列的异步处理适用于高并发、高吞吐量的场景；数据库事务适用于需要确保操作原子性的场景。每种方案都有其优缺点，需要综合考虑性能、复杂度和一致性要求。
