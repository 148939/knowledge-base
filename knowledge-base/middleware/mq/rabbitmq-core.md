# RabbitMQ 核心基础

## 1. 核心概述（官方定义）

RabbitMQ 是一个开源的消息代理软件（消息队列中间件），它实现了高级消息队列协议（AMQP）。RabbitMQ 服务器用 Erlang 语言编写，支持多种客户端，如 Python、Java、Go、Ruby、PHP、C#、JavaScript 等。

### 核心概念
- **生产者（Producer）**：发送消息的应用程序
- **消费者（Consumer）**：接收消息的应用程序
- **队列（Queue）**：存储消息的容器
- **交换机（Exchange）**：接收生产者发送的消息并将消息路由到队列
- **绑定（Binding）**：交换机和队列之间的关联关系
- **虚拟主机（VHost）**：提供资源隔离的独立命名空间

---

## 2. 核心知识点（结构化层级）

### 2.1 消息模型

#### 简单队列模型
- 一个生产者
- 一个队列
- 一个消费者

#### Work Queue 模型
- 一个生产者
- 一个队列
- 多个消费者
- 消息轮询分发

#### Publish/Subscribe 模型
- 一个生产者
- 一个交换机
- 多个队列
- 多个消费者
- 广播消息

#### Routing 模型
- 使用 direct 交换机
- 根据 routing key 路由
- 精确匹配

#### Topics 模型
- 使用 topic 交换机
- 通配符匹配
- * 匹配一个词
- # 匹配零个或多个词

#### RPC 模型
- 远程过程调用
- 发送请求
- 接收响应

### 2.2 交换机类型

#### Direct Exchange
- 精确匹配 routing key
- 适合单播消息

#### Fanout Exchange
- 忽略 routing key
- 广播到所有绑定的队列
- 适合广播消息

#### Topic Exchange
- 通配符匹配 routing key
- 灵活的路由规则
- 适合多播消息

#### Headers Exchange
- 根据消息头匹配
- 忽略 routing key
- 复杂的匹配规则

### 2.3 消息确认

#### 生产者确认
- ConfirmCallback
- ReturnCallback
- 确保消息到达交换机

#### 消费者确认
- 自动确认（autoAck=true）
- 手动确认（basicAck）
- 拒绝消息（basicNack）
- 拒绝并重新入队

### 2.4 消息持久化

#### 交换机持久化
- durable=true
- 交换机重启后保留

#### 队列持久化
- durable=true
- 队列重启后保留

#### 消息持久化
- deliveryMode=2
- 消息写入磁盘
- 重启后不丢失

### 2.5 死信队列

#### 死信原因
- 消息被拒绝且不重新入队
- 消息 TTL 过期
- 队列达到最大长度

#### 死信交换机
- x-dead-letter-exchange
- x-dead-letter-routing-key

### 2.6 延迟队列

#### 实现方式
- 使用 TTL + 死信队列
- 使用插件（rabbitmq_delayed_message_exchange）

#### 应用场景
- 订单超时取消
- 定时任务
- 延迟通知

### 2.7 消息幂等性

#### 幂等性概念
- 多次操作结果一致
- 避免重复消费

#### 实现方式
- 唯一消息 ID
- 数据库唯一约束
-  Redis 去重

---

## 3. 官方标准语法 / API

### 3.1 Java 客户端（Spring AMQP）

```java
// 配置类
@Configuration
public class RabbitMQConfig {
    
    @Bean
    public Queue simpleQueue() {
        return new Queue("simple.queue", true);
    }
    
    @Bean
    public DirectExchange directExchange() {
        return new DirectExchange("direct.exchange", true, false);
    }
    
    @Bean
    public Binding binding() {
        return BindingBuilder
            .bind(simpleQueue())
            .to(directExchange())
            .with("routing.key");
    }
    
    @Bean
    public TopicExchange topicExchange() {
        return new TopicExchange("topic.exchange", true, false);
    }
    
    @Bean
    public Queue topicQueue1() {
        return new Queue("topic.queue1", true);
    }
    
    @Bean
    public Binding topicBinding1() {
        return BindingBuilder
            .bind(topicQueue1())
            .to(topicExchange())
            .with("user.*");
    }
    
    @Bean
    public Queue topicQueue2() {
        return new Queue("topic.queue2", true);
    }
    
    @Bean
    public Binding topicBinding2() {
        return BindingBuilder
            .bind(topicQueue2())
            .to(topicExchange())
            .with("order.#");
    }
    
    @Bean
    public FanoutExchange fanoutExchange() {
        return new FanoutExchange("fanout.exchange", true, false);
    }
    
    @Bean
    public Queue fanoutQueue1() {
        return new Queue("fanout.queue1", true);
    }
    
    @Bean
    public Queue fanoutQueue2() {
        return new Queue("fanout.queue2", true);
    }
    
    @Bean
    public Binding fanoutBinding1() {
        return BindingBuilder
            .bind(fanoutQueue1())
            .to(fanoutExchange());
    }
    
    @Bean
    public Binding fanoutBinding2() {
        return BindingBuilder
            .bind(fanoutQueue2())
            .to(fanoutExchange());
    }
}

// 生产者
@Service
public class MessageProducer {
    
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void sendSimpleMessage(String message) {
        rabbitTemplate.convertAndSend("simple.queue", message);
    }
    
    public void sendDirectMessage(String routingKey, String message) {
        rabbitTemplate.convertAndSend("direct.exchange", routingKey, message);
    }
    
    public void sendTopicMessage(String routingKey, String message) {
        rabbitTemplate.convertAndSend("topic.exchange", routingKey, message);
    }
    
    public void sendFanoutMessage(String message) {
        rabbitTemplate.convertAndSend("fanout.exchange", "", message);
    }
    
    public void sendPersistentMessage(String message) {
        rabbitTemplate.convertAndSend(
            "simple.queue", 
            message,
            msg -> {
                msg.getMessageProperties().setDeliveryMode(MessageDeliveryMode.PERSISTENT);
                return msg;
            }
        );
    }
}

// 消费者
@Service
public class MessageConsumer {
    
    @RabbitListener(queues = "simple.queue")
    public void handleSimpleMessage(String message) {
        System.out.println("Received: " + message);
    }
    
    @RabbitListener(queues = "topic.queue1")
    public void handleTopicMessage1(String message) {
        System.out.println("Topic Queue 1 Received: " + message);
    }
    
    @RabbitListener(queues = "topic.queue2")
    public void handleTopicMessage2(String message) {
        System.out.println("Topic Queue 2 Received: " + message);
    }
    
    @RabbitListener(queues = "fanout.queue1")
    public void handleFanoutMessage1(String message) {
        System.out.println("Fanout Queue 1 Received: " + message);
    }
    
    @RabbitListener(queues = "fanout.queue2")
    public void handleFanoutMessage2(String message) {
        System.out.println("Fanout Queue 2 Received: " + message);
    }
    
    @RabbitListener(queues = "simple.queue")
    public void handleMessageWithAck(String message, Channel channel, 
                                     @Header(AmqpHeaders.DELIVERY_TAG) long deliveryTag) 
                                     throws IOException {
        try {
            System.out.println("Processing: " + message);
            // 手动确认
            channel.basicAck(deliveryTag, false);
        } catch (Exception e) {
            // 拒绝消息，重新入队
            channel.basicNack(deliveryTag, false, true);
        }
    }
}
```

### 3.2 Python 客户端（pika）

```python
import pika

# 连接
connection = pika.BlockingConnection(
    pika.ConnectionParameters('localhost')
)
channel = connection.channel()

# 声明队列
channel.queue_declare(queue='hello', durable=True)

# 发送消息
channel.basic_publish(
    exchange='',
    routing_key='hello',
    body='Hello World!',
    properties=pika.BasicProperties(
        delivery_mode=2  # 持久化
    )
)

print(" [x] Sent 'Hello World!'")
connection.close()

# 消费者
def callback(ch, method, properties, body):
    print(f" [x] Received {body.decode()}")
    # 手动确认
    ch.basic_ack(delivery_tag=method.delivery_tag)

channel.basic_consume(
    queue='hello',
    on_message_callback=callback,
    auto_ack=False
)

print(' [*] Waiting for messages. To exit press CTRL+C')
channel.start_consuming()

# Publish/Subscribe
channel.exchange_declare(exchange='logs', exchange_type='fanout')

result = channel.queue_declare(queue='', exclusive=True)
queue_name = result.method.queue

channel.queue_bind(exchange='logs', queue=queue_name)

def fanout_callback(ch, method, properties, body):
    print(f" [x] {body.decode()}")

channel.basic_consume(
    queue=queue_name,
    on_message_callback=fanout_callback,
    auto_ack=True
)

# Routing
channel.exchange_declare(exchange='direct_logs', exchange_type='direct')

severity = 'error'
channel.queue_bind(
    exchange='direct_logs',
    queue=queue_name,
    routing_key=severity
)

# Topics
channel.exchange_declare(exchange='topic_logs', exchange_type='topic')

routing_key = 'kern.info'
channel.queue_bind(
    exchange='topic_logs',
    queue=queue_name,
    routing_key='kern.*'
)
```

### 3.3 Go 客户端（streadway/amqp）

```go
package main

import (
    "log"
    "github.com/streadway/amqp"
)

func failOnError(err error, msg string) {
    if err != nil {
        log.Fatalf("%s: %s", msg, err)
    }
}

func main() {
    // 连接
    conn, err := amqp.Dial("amqp://guest:guest@localhost:5672/")
    failOnError(err, "Failed to connect to RabbitMQ")
    defer conn.Close()

    ch, err := conn.Channel()
    failOnError(err, "Failed to open a channel")
    defer ch.Close()

    // 声明队列
    q, err := ch.QueueDeclare(
        "hello",    // name
        true,       // durable
        false,      // delete when unused
        false,      // exclusive
        false,      // no-wait
        nil,        // arguments
    )
    failOnError(err, "Failed to declare a queue")

    // 发送消息
    body := "Hello World!"
    err = ch.Publish(
        "",         // exchange
        q.Name,     // routing key
        false,      // mandatory
        false,      // immediate
        amqp.Publishing{
            DeliveryMode: amqp.Persistent,
            ContentType:  "text/plain",
            Body:         []byte(body),
        })
    failOnError(err, "Failed to publish a message")
    log.Printf(" [x] Sent %s", body)

    // 消费者
    msgs, err := ch.Consume(
        q.Name,     // queue
        "",         // consumer
        false,      // auto-ack
        false,      // exclusive
        false,      // no-local
        false,      // no-wait
        nil,        // args
    )
    failOnError(err, "Failed to register a consumer")

    forever := make(chan bool)

    go func() {
        for d := range msgs {
            log.Printf("Received a message: %s", d.Body)
            d.Ack(false) // 手动确认
        }
    }()

    log.Printf(" [*] Waiting for messages. To exit press CTRL+C")
    <-forever
}
```

---

## 4. 官方示例代码（可运行）

### 4.1 订单超时取消（延迟队列 + 死信队列）

```java
@Configuration
public class DelayOrderConfig {
    
    // 订单队列（带 TTL）
    @Bean
    public Queue orderDelayQueue() {
        Map<String, Object> args = new HashMap<>();
        args.put("x-dead-letter-exchange", "order.dlx.exchange");
        args.put("x-dead-letter-routing-key", "order.dlx");
        args.put("x-message-ttl", 30000); // 30秒
        return new Queue("order.delay.queue", true, false, false, args);
    }
    
    // 订单交换机
    @Bean
    public DirectExchange orderDelayExchange() {
        return new DirectExchange("order.delay.exchange", true, false);
    }
    
    @Bean
    public Binding orderDelayBinding() {
        return BindingBuilder
            .bind(orderDelayQueue())
            .to(orderDelayExchange())
            .with("order.delay");
    }
    
    // 死信队列
    @Bean
    public Queue orderDlxQueue() {
        return new Queue("order.dlx.queue", true);
    }
    
    // 死信交换机
    @Bean
    public DirectExchange orderDlxExchange() {
        return new DirectExchange("order.dlx.exchange", true, false);
    }
    
    @Bean
    public Binding orderDlxBinding() {
        return BindingBuilder
            .bind(orderDlxQueue())
            .to(orderDlxExchange())
            .with("order.dlx");
    }
}

@Service
public class OrderService {
    
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void createOrder(Order order) {
        // 保存订单
        orderMapper.insert(order);
        
        // 发送延迟消息
        rabbitTemplate.convertAndSend(
            "order.delay.exchange",
            "order.delay",
            order.getId()
        );
    }
}

@Service
public class OrderTimeoutHandler {
    
    @Autowired
    private OrderMapper orderMapper;
    
    @RabbitListener(queues = "order.dlx.queue")
    public void handleOrderTimeout(Long orderId) {
        Order order = orderMapper.selectById(orderId);
        if (order != null && "PENDING".equals(order.getStatus())) {
            // 取消订单
            order.setStatus("CANCELLED");
            orderMapper.updateById(order);
            System.out.println("Order " + orderId + " cancelled due to timeout");
        }
    }
}
```

### 4.2 消息幂等性处理（Redis）

```java
@Service
public class IdempotentMessageHandler {
    
    @Autowired
    private StringRedisTemplate redisTemplate;
    
    private static final String MESSAGE_ID_PREFIX = "msg:id:";
    private static final long MESSAGE_TTL = 24 * 60 * 60; // 24小时
    
    @RabbitListener(queues = "order.queue")
    public void handleOrderMessage(OrderMessage message, 
                                    Channel channel,
                                    @Header(AmqpHeaders.DELIVERY_TAG) long deliveryTag) 
                                    throws IOException {
        String messageId = message.getMessageId();
        
        // 检查消息是否已处理
        String key = MESSAGE_ID_PREFIX + messageId;
        Boolean isNew = redisTemplate.opsForValue().setIfAbsent(key, "1", MESSAGE_TTL, TimeUnit.SECONDS);
        
        if (Boolean.TRUE.equals(isNew)) {
            try {
                // 处理消息
                processOrder(message);
                channel.basicAck(deliveryTag, false);
            } catch (Exception e) {
                // 处理失败，删除幂等性标记
                redisTemplate.delete(key);
                channel.basicNack(deliveryTag, false, true);
            }
        } else {
            // 消息已处理，直接确认
            channel.basicAck(deliveryTag, false);
            System.out.println("Duplicate message: " + messageId);
        }
    }
    
    private void processOrder(OrderMessage message) {
        // 业务逻辑
        System.out.println("Processing order: " + message.getOrderId());
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 消息可靠性

1. **生产者确认**
   - 启用 ConfirmCallback
   - 启用 ReturnCallback
   - 处理发送失败

2. **消息持久化**
   - 交换机持久化
   - 队列持久化
   - 消息持久化

3. **消费者确认**
   - 手动确认消息
   - 合理处理异常
   - 避免消息丢失

### 5.2 性能优化

1. **批量发送**
   - 使用批量发送
   - 减少网络开销
   - 提高吞吐量

2. **Prefetch Count**
   - 合理设置 prefetch
   - 避免消费者过载
   - 提高消息处理效率

3. **队列优化**
   - 避免队列过长
   - 合理设置 TTL
   - 使用死信队列

### 5.3 安全最佳实践

1. **认证授权**
   - 使用强密码
   - 配置用户权限
   - 使用虚拟主机隔离

2. **网络安全**
   - 使用 TLS 加密
   - 限制访问 IP
   - 使用 VPN

3. **监控告警**
   - 监控队列长度
   - 监控消息堆积
   - 设置告警阈值

### 5.4 常见问题处理

1. **消息堆积**
   - 增加消费者
   - 优化消费逻辑
   - 临时扩容

2. **消息重复**
   - 实现幂等性
   - 使用唯一 ID
   - 数据库唯一约束

3. **消息丢失**
   - 启用确认机制
   - 消息持久化
   - 监控发送状态

---

## 6. 官方文档参考链接

- [RabbitMQ 官方文档](https://www.rabbitmq.com/documentation.html)
- [RabbitMQ 教程](https://www.rabbitmq.com/getstarted.html)
- [Spring AMQP 文档](https://spring.io/projects/spring-amqp)
- [pika Python 客户端](https://pika.readthedocs.io/)
- [streadway/amqp Go 客户端](https://pkg.go.dev/github.com/streadway/amqp)
- [RabbitMQ 管理 API](https://rawcdn.githack.com/rabbitmq/rabbitmq-server/v3.12.0/deps/rabbitmq_management/priv/www/api/index.html)
