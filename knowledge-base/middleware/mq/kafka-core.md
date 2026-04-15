# Kafka 核心基础

## 1. 核心概述（官方定义）

Apache Kafka 是一个开源的分布式事件流平台，用于高性能数据管道、流分析、数据集成和关键任务应用。Kafka 最初由 LinkedIn 开发，后来成为 Apache 顶级项目。

### 核心概念
- **Producer**：生产者，向 Kafka 发送消息的客户端
- **Consumer**：消费者，从 Kafka 读取消息的客户端
- **Broker**：Kafka 集群中的服务器节点
- **Topic**：主题，消息的分类
- **Partition**：分区，Topic 的物理分组
- **Offset**：偏移量，消息在分区中的位置
- **Consumer Group**：消费者组，一组共同消费的消费者

---

## 2. 核心知识点（结构化层级）

### 2.1 Kafka 架构

#### Broker
- 存储消息
- 处理请求
- 副本管理
- 控制器选举

#### ZooKeeper/KRaft
- 元数据管理
- 集群协调
- 配置管理
- 健康检查

#### Producer
- 发送消息
- 分区选择
- 消息序列化
- 消息压缩

#### Consumer
- 拉取消息
- 消费位置提交
- 消费者组协调
- 消息反序列化

### 2.2 Topic 与 Partition

#### Topic
- 消息分类
- 逻辑概念
- 可配置保留策略
- 可配置副本数

#### Partition
- 物理存储单元
- 顺序写入
- 并行消费
- 消息有序性

#### 分区策略
- 轮询（Round Robin）
- 键哈希（Key Hash）
- 自定义分区器

### 2.3 消息生产

#### 发送模式
- 发后即忘
- 同步发送
- 异步发送

#### 确认机制
- acks=0：不等待确认
- acks=1：等待 Leader 确认
- acks=all：等待所有 ISR 确认

#### 消息可靠性
- 重试机制
- 幂等性
- 事务支持

### 2.4 消息消费

#### 消费模式
- 主动拉取（Pull）
- 长轮询（Long Polling）

#### 消费位置
- 自动提交
- 手动提交
- 特定位置消费

#### 消费者组
- 组协调器
- 分区分配
- 重平衡（Rebalance）

### 2.5 副本机制

#### 副本类型
- Leader 副本
- Follower 副本
- ISR（In-Sync Replicas）

#### 副本同步
- HW（High Watermark）
- LEO（Log End Offset）
- 副本同步策略

#### 故障转移
- Leader 选举
- ISR 收缩与扩张
- 副本迁移

### 2.6 日志存储

#### 日志结构
- Segment 文件
- Index 文件
- Time Index 文件

#### 日志清理
- 基于时间
- 基于大小
- 日志压缩（Log Compaction）

#### 索引机制
- 稀疏索引
- 偏移量索引
- 时间戳索引

### 2.7 流处理

#### Kafka Streams
- 流处理 DSL
- 处理器 API
- 状态存储
- 窗口操作

#### KSQL
- 流式 SQL
- 表和流
- 持续查询
- 连接和聚合

---

## 3. 官方标准语法 / API

### 3.1 Java 生产者

```java
import org.apache.kafka.clients.producer.KafkaProducer;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.clients.producer.RecordMetadata;
import java.util.Properties;

public class ProducerExample {
    
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        props.put("acks", "all");
        props.put("retries", 3);
        props.put("batch.size", 16384);
        props.put("linger.ms", 1);
        props.put("buffer.memory", 33554432);
        
        KafkaProducer<String, String> producer = new KafkaProducer<>(props);
        
        try {
            for (int i = 0; i < 10; i++) {
                ProducerRecord<String, String> record = 
                    new ProducerRecord<>("my-topic", "key-" + i, "value-" + i);
                
                // 异步发送
                producer.send(record, (metadata, exception) -> {
                    if (exception == null) {
                        System.out.println("Sent: " + metadata.offset());
                    } else {
                        exception.printStackTrace();
                    }
                });
                
                // 同步发送
                RecordMetadata metadata = producer.send(record).get();
                System.out.println("Sync sent: " + metadata.offset());
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            producer.close();
        }
    }
}

// 自定义分区器
import org.apache.kafka.clients.producer.Partitioner;
import org.apache.kafka.common.Cluster;
import java.util.Map;

public class CustomPartitioner implements Partitioner {
    
    @Override
    public int partition(String topic, Object key, byte[] keyBytes, 
                          Object value, byte[] valueBytes, Cluster cluster) {
        int numPartitions = cluster.partitionsForTopic(topic).size();
        return Math.abs(key.hashCode()) % numPartitions;
    }
    
    @Override
    public void close() {}
    
    @Override
    public void configure(Map<String, ?> configs) {}
}
```

### 3.2 Java 消费者

```java
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import java.time.Duration;
import java.util.Arrays;
import java.util.Properties;

public class ConsumerExample {
    
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put("bootstrap.servers", "localhost:9092");
        props.put("group.id", "test-group");
        props.put("enable.auto.commit", "true");
        props.put("auto.commit.interval.ms", "1000");
        props.put("key.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        props.put("value.deserializer", "org.apache.kafka.common.serialization.StringDeserializer");
        props.put("auto.offset.reset", "earliest");
        
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(props);
        consumer.subscribe(Arrays.asList("my-topic"));
        
        try {
            while (true) {
                ConsumerRecords<String, String> records = 
                    consumer.poll(Duration.ofMillis(100));
                
                for (ConsumerRecord<String, String> record : records) {
                    System.out.printf("offset = %d, key = %s, value = %s%n",
                        record.offset(), record.key(), record.value());
                }
                
                // 手动提交
                consumer.commitSync();
                // 异步提交
                consumer.commitAsync();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            consumer.close();
        }
    }
}

// 手动指定分区
consumer.assign(Arrays.asList(
    new TopicPartition("my-topic", 0),
    new TopicPartition("my-topic", 1)
));

// 从特定偏移量开始
TopicPartition partition = new TopicPartition("my-topic", 0);
consumer.assign(Arrays.asList(partition));
consumer.seek(partition, 100);

// 从开头开始
consumer.seekToBeginning(Arrays.asList(partition));

// 从末尾开始
consumer.seekToEnd(Arrays.asList(partition));
```

### 3.3 命令行工具

```bash
# 启动 ZooKeeper
bin/zookeeper-server-start.sh config/zookeeper.properties

# 启动 Kafka
bin/kafka-server-start.sh config/server.properties

# 创建 Topic
bin/kafka-topics.sh --create \
    --bootstrap-server localhost:9092 \
    --replication-factor 1 \
    --partitions 3 \
    --topic my-topic

# 查看 Topic 列表
bin/kafka-topics.sh --list \
    --bootstrap-server localhost:9092

# 查看 Topic 详情
bin/kafka-topics.sh --describe \
    --bootstrap-server localhost:9092 \
    --topic my-topic

# 发送消息
bin/kafka-console-producer.sh \
    --broker-list localhost:9092 \
    --topic my-topic

# 消费消息
bin/kafka-console-consumer.sh \
    --bootstrap-server localhost:9092 \
    --topic my-topic \
    --from-beginning

# 查看消费者组
bin/kafka-consumer-groups.sh \
    --bootstrap-server localhost:9092 \
    --list

# 查看消费者组详情
bin/kafka-consumer-groups.sh \
    --bootstrap-server localhost:9092 \
    --describe \
    --group test-group

# 删除 Topic
bin/kafka-topics.sh --delete \
    --bootstrap-server localhost:9092 \
    --topic my-topic

# 修改 Topic 分区数
bin/kafka-topics.sh --alter \
    --bootstrap-server localhost:9092 \
    --topic my-topic \
    --partitions 5
```

### 3.4 Kafka Streams

```java
import org.apache.kafka.common.serialization.Serdes;
import org.apache.kafka.streams.KafkaStreams;
import org.apache.kafka.streams.StreamsBuilder;
import org.apache.kafka.streams.StreamsConfig;
import org.apache.kafka.streams.kstream.KStream;
import org.apache.kafka.streams.kstream.KTable;
import java.util.Properties;

public class WordCountExample {
    
    public static void main(String[] args) {
        Properties props = new Properties();
        props.put(StreamsConfig.APPLICATION_ID_CONFIG, "wordcount-app");
        props.put(StreamsConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(StreamsConfig.DEFAULT_KEY_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        props.put(StreamsConfig.DEFAULT_VALUE_SERDE_CLASS_CONFIG, Serdes.String().getClass().getName());
        
        StreamsBuilder builder = new StreamsBuilder();
        
        KStream<String, String> source = builder.stream("input-topic");
        
        KTable<String, Long> wordCounts = source
            .flatMapValues(value -> Arrays.asList(value.toLowerCase().split("\\W+")))
            .groupBy((key, word) -> word)
            .count();
        
        wordCounts.toStream().to("output-topic");
        
        KafkaStreams streams = new KafkaStreams(builder.build(), props);
        streams.start();
        
        Runtime.getRuntime().addShutdownHook(new Thread(streams::close));
    }
}
```

---

## 4. 官方示例代码（可运行）

### 4.1 订单处理系统

```java
// 订单生产者
@Service
public class OrderProducer {
    
    @Autowired
    private KafkaTemplate<String, Order> kafkaTemplate;
    
    public void sendOrder(Order order) {
        ProducerRecord<String, Order> record = 
            new ProducerRecord<>("order-topic", order.getId(), order);
        
        kafkaTemplate.send(record).addCallback(
            result -> {
                System.out.println("Order sent: " + order.getId());
            },
            ex -> {
                System.err.println("Failed to send order: " + ex.getMessage());
            }
        );
    }
}

// 订单消费者
@Service
public class OrderConsumer {
    
    @KafkaListener(topics = "order-topic", groupId = "order-group")
    public void handleOrder(ConsumerRecord<String, Order> record,
                            @Header(KafkaHeaders.RECEIVED_PARTITION) int partition,
                            @Header(KafkaHeaders.OFFSET) long offset) {
        Order order = record.value();
        System.out.println("Received order: " + order.getId());
        System.out.println("Partition: " + partition + ", Offset: " + offset);
        
        processOrder(order);
    }
    
    private void processOrder(Order order) {
        // 业务逻辑
        orderService.save(order);
        inventoryService.reserve(order);
        paymentService.process(order);
    }
}

// 配置类
@Configuration
public class KafkaConfig {
    
    @Bean
    public ProducerFactory<String, Order> producerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        props.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        props.put(ProducerConfig.ACKS_CONFIG, "all");
        props.put(ProducerConfig.RETRIES_CONFIG, 3);
        props.put(ProducerConfig.ENABLE_IDEMPOTENCE_CONFIG, true);
        return new DefaultKafkaProducerFactory<>(props);
    }
    
    @Bean
    public KafkaTemplate<String, Order> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
    
    @Bean
    public ConsumerFactory<String, Order> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "order-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        props.put(ConsumerConfig.AUTO_OFFSET_RESET_CONFIG, "earliest");
        props.put(ConsumerConfig.ENABLE_AUTO_COMMIT_CONFIG, false);
        return new DefaultKafkaConsumerFactory<>(
            props,
            new StringDeserializer(),
            new JsonDeserializer<>(Order.class)
        );
    }
    
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Order> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Order> factory = 
            new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        factory.getContainerProperties().setAckMode(ContainerProperties.AckMode.MANUAL_IMMEDIATE);
        return factory;
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 生产者最佳实践

1. **发送确认**
   - 根据业务需求选择 acks
   - 生产环境使用 acks=all
   - 合理设置重试次数

2. **批量发送**
   - 配置 batch.size
   - 配置 linger.ms
   - 减少网络开销

3. **消息压缩**
   - 启用消息压缩
   - 选择合适的压缩算法
   - 减少存储和网络成本

4. **幂等性**
   - 启用幂等生产者
   - 使用事务
   - 业务层保证幂等

### 5.2 消费者最佳实践

1. **消费位置管理**
   - 合理选择自动/手动提交
   - 避免重复消费
   - 处理消费失败

2. **消费者组**
   - 合理设置组 ID
   - 分区数与消费者数匹配
   - 处理重平衡

3. **批量消费**
   - 配置 fetch.min.bytes
   - 配置 fetch.max.wait.ms
   - 提高消费效率

4. **异常处理**
   - 捕获和处理异常
   - 死信队列
   - 监控和告警

### 5.3 运维最佳实践

1. **监控**
   - 监控 Broker 状态
   - 监控 Topic 指标
   - 监控消费者 lag
   - 设置告警阈值

2. **扩容**
   - 合理规划分区数
   - 按需扩容 Broker
   - 合理设置副本数

3. **数据保留**
   - 合理设置保留策略
   - 定期清理旧数据
   - 使用日志压缩

4. **安全**
   - 配置认证和授权
   - 启用 TLS 加密
   - 限制访问权限

---

## 6. 官方文档参考链接

- [Apache Kafka 官方文档](https://kafka.apache.org/documentation/)
- [Kafka 入门指南](https://kafka.apache.org/quickstart)
- [Spring Kafka 文档](https://spring.io/projects/spring-kafka)
- [Kafka Streams 文档](https://kafka.apache.org/documentation/streams/)
- [Kafka 配置文档](https://kafka.apache.org/documentation/#configuration)
- [Kafka 运维指南](https://kafka.apache.org/documentation/#operations)
