### 1. 消息持久化
- **同步刷盘（SYNC_FLUSH）**：消息写入后，立即同步刷盘，确保消息持久化到磁盘。同步刷盘虽然会增加延迟，但极大地提高了消息的可靠性。
```
flushDiskType=SYNC_FLUSH
```

- **异步刷盘（ASYNC_FLUSH）**：消息写入后，异步刷盘，性能较高，但可靠性略低于同步刷盘。
```
flushDiskType=ASYNC_FLUSH
```
### 2. 主从复制
RocketMQ 支持主从复制，通过将消息从主节点复制到从节点来提高消息的可靠性。

- **同步复制（SYNC_MASTER）**：消息写入主节点后，立即同步复制到从节点，确保消息在主从节点都存在。同步复制提高了消息的可靠性，但会增加写入延迟。
```
brokerRole=SYNC_MASTER
```

- **异步复制（ASYNC_MASTER）**：消息写入主节点后，异步复制到从节点，性能较高，但可靠性略低于同步复制。
```
brokerRole=ASYNC_MASTER
```
### 3. 消息确认机制
RocketMQ 提供了消息确认机制，确保消息被消费者成功处理。

- **消费者确认**：消费者处理完消息后，向 RocketMQ 发送确认（ack）。如果消费者未确认消息（如消费者崩溃），RocketMQ 会将消息重新投递给其他消费者。
```
// 消费者处理消息
consumer.registerMessageListener(newMessageListenerConcurrently() {
    @Override
    public ConsumeConcurrentlyStatus consumeMessage(List<MessageExt> msgs, ConsumeConcurrentlyContext context) {
        // 处理消息逻辑
        return ConsumeConcurrentlyStatus.CONSUME_SUCCESS; // 处理成功
    }
});
```
### 4. 分布式事务
RocketMQ 支持分布式事务，确保消息和业务操作的一致性。通过事务消息机制，RocketMQ 可以在业务操作和消息发送之间建立事务关系，确保消息不丢失。
```
TransactionMQProducer producer = new TransactionMQProducer("producer_group");
producer.start();

Message msg = new Message("topic", "tag", "key", "body".getBytes());
SendResult sendResult = producer.sendMessageInTransaction(msg, new LocalTransactionExecuter() {
    @Override
    public LocalTransactionState executeLocalTransactionBranch(Message msg, Object arg) {
        // 执行本地事务逻辑
        return LocalTransactionState.COMMIT_MESSAGE; // 提交事务
    }
}, null);
```
### 5. 重试机制
RocketMQ 提供了消息重试机制，确保消息在处理失败时不会丢失。

- **生产者重试**：生产者在消息发送失败时，可以进行重试，确保消息成功发送。
```
producer.setRetryTimesWhenSendFailed(3); // 设置重试次数
```

- **消费者重试**：消费者在处理消息失败时，RocketMQ 会自动进行重试，直到消息被成功处理或达到最大重试次数。
```
consumer.setMaxReconsumeTimes(5); // 设置最大重试次数
```
### 6. 死信队列（DLQ）
RocketMQ 支持死信队列，当消息达到最大重试次数仍未被成功处理时，会被投递到死信队列，便于后续处理。
```
consumer.setMaxReconsumeTimes(5); // 设置最大重试次数
// 消息达到最大重试次数后，将被投递到死信队列
```
### 7. 消息顺序
RocketMQ 支持消息顺序消费，确保消息按照发送顺序被消费，避免因乱序导致的消息丢失或处理错误。
```
consumer.registerMessageListener(new MessageListenerOrderly() {
    @Override
    public ConsumeOrderlyStatus consumeMessage(List<MessageExt> msgs, ConsumeOrderlyContext context) {
        // 顺序处理消息逻辑
        return ConsumeOrderlyStatus.SUCCESS; // 处理成功
    }
});
```
### 
