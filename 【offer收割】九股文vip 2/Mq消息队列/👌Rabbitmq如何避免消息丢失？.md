### 1. 消息持久化
- **持久化队列**：创建队列时，可以将其声明为持久化队列（durable）。持久化队列在 RabbitMQ 重启后依然存在。
```
channel.queueDeclare("queue_name", true, false, false, null);
```

- **持久化消息**：在发送消息时，可以将消息标记为持久化（persistent）。持久化消息会被写入磁盘，即使 RabbitMQ 重启，消息也不会丢失。
```
AMQP.BasicProperties props = new AMQP.BasicProperties.Builder()
    .deliveryMode(2) // 2 表示持久化消息
    .build();
channel.basicPublish("exchange_name", "routing_key", props, messageBody);
```
### 2. 消息确认机制（Acknowledgements）

- **消费者确认**：消费者处理完消息后，向 RabbitMQ 发送确认（ack）。如果消费者未确认消息（如消费者崩溃），RabbitMQ 会将消息重新投递给其他消费者。
```
// 自动确认关闭
channel.basicConsume("queue_name", false, consumer);

// 消费者处理完消息后手动确认
channel.basicAck(deliveryTag, false);
```

- **发布者确认**：发布者可以启用发布确认模式（publisher confirms），RabbitMQ 会在消息成功写入队列后发送确认给发布者。
```
channel.confirmSelect();
channel.basicPublish("exchange_name", "routing_key", null, messageBody);
channel.waitForConfirmsOrDie(); // 等待确认
```
### 3. 事务支持
RabbitMQ 支持 AMQP 事务，可以在一个事务中发布多条消息，确保这些消息要么全部成功，要么全部失败。
```
channel.txSelect();
channel.basicPublish("exchange_name", "routing_key", null, messageBody);
channel.txCommit(); // 提交事务
```
### 4. 镜像队列（Mirrored Queues）
RabbitMQ 的集群模式支持镜像队列（也称为高可用队列），可以将队列的数据复制到多个节点上，确保在一个节点故障时，其他节点仍然可以提供服务。
```
// 定义队列策略，将队列配置为镜像队列
rabbitmqctl set_policy ha-all "^queue_name$" '{"ha-mode":"all"}'
```
### 5. 死信队列（Dead Letter Exchanges）
RabbitMQ 支持死信队列（DLX），当消息在队列中被拒绝、过期或达到最大重试次数时，可以将消息发送到死信交换器，便于后续处理。
```
// 声明队列时指定死信交换器
Map<String, Object> args = new HashMap<>();
args.put("x-dead-letter-exchange", "dlx_exchange");
channel.queueDeclare("queue_name", true, false, false, args);
```
### 6. 消费者重试机制
消费者可以实现重试机制，当消息处理失败时，可以将消息重新投递到队列或死信队列，避免消息丢失。
```
try {
    // 处理消息
    channel.basicAck(deliveryTag, false);
} catch (Exception e) {
    channel.basicNack(deliveryTag, false, true); // 重试
}
```
### 7. 消息 TTL（Time-To-Live）
消息可以设置 TTL，超过 TTL 的消息会自动被移除或转移到死信队列，避免消息在队列中无限积压。
```
AMQP.BasicProperties props = new AMQP.BasicProperties.Builder()
    .expiration("60000") // 消息 TTL 为 60 秒
    .build();
channel.basicPublish("exchange_name", "routing_key", props, messageBody);
```
# 
