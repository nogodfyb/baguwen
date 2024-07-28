### 1. 使用 Kafka 自带工具
Kafka 提供了一些命令行工具，可以用来查看消息积压情况。
#### kafka-consumer-groups.sh
这个工具可以显示消费者组的消费情况，包括当前的偏移量、日志末尾偏移量和积压消息数量。
```
bin/kafka-consumer-groups.sh --bootstrap-server <broker_address> --describe --group <consumer_group>
```
```
GROUP           TOPIC           PARTITION  CURRENT-OFFSET  LOG-END-OFFSET  LAG             CONSUMER-ID                                    HOST            CLIENT-ID
consumer-group  my_topic        0          1234            2345            1111            consumer-1-12345                               /127.0.0.1      consumer-1
```
LAG列表示消息积压的数量。
### 2. 使用 Kafka Manager
Kafka Manager 是 Yahoo 开源的一个工具，可以用来管理和监控 Kafka 集群。它提供了一个图形用户界面，可以直观地查看消费者组的消费情况和消息积压。
### 3. 使用 Burrow
Burrow 是 LinkedIn 开源的一个 Kafka 消费者监控工具，它可以持续监控消费者组的消费进度，并生成消息积压的报告。
### 4. 使用 Kafka Exporter
Kafka Exporter 是一个 Prometheus 导出器，用于监控 Kafka 集群的状态，包括消息积压情况。它可以将 Kafka 的监控数据导出到 Prometheus，然后可以使用 Grafana 等工具进行可视化。
### 5. 使用自定义脚本
可以编写自定义脚本，使用 Kafka 提供的 API 来获取消息积压情况。可以使用 Kafka 的 AdminClient API 来获取主题分区的日志末尾偏移量，然后与消费者的当前偏移量进行比较。
```
import org.apache.kafka.clients.admin.AdminClient;
import org.apache.kafka.clients.admin.AdminClientConfig;
import org.apache.kafka.clients.admin.ListConsumerGroupOffsetsResult;
import org.apache.kafka.clients.admin.OffsetSpec;
import org.apache.kafka.clients.admin.TopicPartitionOffsetSpec;
import org.apache.kafka.common.TopicPartition;

import java.util.Collections;
import java.util.Map;
import java.util.Properties;
import java.util.concurrent.ExecutionException;

public class KafkaLagChecker {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        Properties props = new Properties();
        props.put(AdminClientConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        try (AdminClient adminClient = AdminClient.create(props)) {
            String groupId = "your-consumer-group";
            ListConsumerGroupOffsetsResult offsetsResult = adminClient.listConsumerGroupOffsets(groupId);
            Map<TopicPartition, Long> logEndOffsets = adminClient.listOffsets(
                    Collections.singletonMap(new TopicPartition("your-topic", 0), OffsetSpec.latest())).all().get();
            
            offsetsResult.partitionsToOffsetAndMetadata().get().forEach((tp, offsetAndMetadata) -> {
                long logEndOffset = logEndOffsets.get(tp);
                long lag = logEndOffset - offsetAndMetadata.offset();
                System.out.printf("Topic: %s, Partition: %d, Lag: %d%n", tp.topic(), tp.partition(), lag);
            });
        }
    }
}
```
### 总结
查看 Kafka 消息积压的常用方法包括使用 Kafka 自带的命令行工具（如kafka-consumer-groups.sh）、第三方监控工具（如 Kafka Manager、Burrow 和 Kafka Exporter）以及编写自定义脚本。
