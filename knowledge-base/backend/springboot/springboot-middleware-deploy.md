# 检索索引：本文档收录【Spring Boot】【中间件与部署】知识点，内容100%来自Spring官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Spring Boot 提供了丰富的中间件集成支持，包括消息队列、缓存、搜索等，同时也提供了多种部署方式。Spring Boot 应用可以打包成可执行的 JAR 或 WAR 文件，方便部署到各种环境中。

Spring Boot 中间件与部署的核心特性：
1. **自动配置**：对常用中间件提供开箱即用的支持
2. **多环境配置**：支持 Profile 管理不同环境的配置
3. **多种部署方式**：支持 JAR、WAR、Docker、Kubernetes 等
4. **监控和管理**：提供 Actuator 进行应用监控

## 2. 核心知识点（结构化层级）

### 2.1 消息队列
1. RabbitMQ 集成
2. Kafka 集成
3. ActiveMQ 集成
4. 消息发送和接收
5. 消息确认机制
6. 消息队列配置

### 2.2 缓存
1. Spring Cache 抽象
2. Redis 缓存
3. Caffeine 缓存
4. EhCache 缓存
5. 缓存注解（@Cacheable、@CachePut、@CacheEvict）

### 2.3 搜索
1. Elasticsearch 集成
2. Spring Data Elasticsearch
3. 索引和文档操作
4. 搜索查询

### 2.4 配置管理
1. Profile 配置
2. 外部化配置
3. 配置文件优先级
4. 配置加密

### 2.5 应用监控
1. Spring Boot Actuator
2. 健康检查
3. 指标监控
4. 日志管理

### 2.6 部署方式
1. JAR 包部署
2. WAR 包部署
3. Docker 容器化
4. Kubernetes 部署
5. 云平台部署

## 3. 官方标准语法 / API

### RabbitMQ 集成配置
```java
import org.springframework.amqp.core.*;
import org.springframework.amqp.rabbit.connection.ConnectionFactory;
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.amqp.rabbit.listener.SimpleMessageListenerContainer;
import org.springframework.amqp.support.converter.Jackson2JsonMessageConverter;
import org.springframework.amqp.support.converter.MessageConverter;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class RabbitMQConfig {
    
    public static final String QUEUE_NAME = "my-queue";
    public static final String EXCHANGE_NAME = "my-exchange";
    public static final String ROUTING_KEY = "my-routing-key";
    
    @Bean
    public Queue queue() {
        return new Queue(QUEUE_NAME, true);
    }
    
    @Bean
    public DirectExchange exchange() {
        return new DirectExchange(EXCHANGE_NAME);
    }
    
    @Bean
    public Binding binding(Queue queue, DirectExchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with(ROUTING_KEY);
    }
    
    @Bean
    public MessageConverter jsonMessageConverter() {
        return new Jackson2JsonMessageConverter();
    }
    
    @Bean
    public RabbitTemplate rabbitTemplate(ConnectionFactory connectionFactory) {
        RabbitTemplate rabbitTemplate = new RabbitTemplate(connectionFactory);
        rabbitTemplate.setMessageConverter(jsonMessageConverter());
        return rabbitTemplate;
    }
}

// 消息发送服务
import org.springframework.amqp.rabbit.core.RabbitTemplate;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MessageProducer {
    
    @Autowired
    private RabbitTemplate rabbitTemplate;
    
    public void sendMessage(String message) {
        rabbitTemplate.convertAndSend(
            RabbitMQConfig.EXCHANGE_NAME,
            RabbitMQConfig.ROUTING_KEY,
            message
        );
    }
    
    public void sendObject(Object object) {
        rabbitTemplate.convertAndSend(
            RabbitMQConfig.EXCHANGE_NAME,
            RabbitMQConfig.ROUTING_KEY,
            object
        );
    }
}

// 消息监听服务
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

@Service
public class MessageConsumer {
    
    @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
    public void handleMessage(String message) {
        System.out.println("Received message: " + message);
    }
    
    @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
    public void handleObject(Object object) {
        System.out.println("Received object: " + object);
    }
}
```

### Kafka 集成配置
```java
import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.apache.kafka.common.serialization.StringSerializer;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.kafka.annotation.EnableKafka;
import org.springframework.kafka.config.ConcurrentKafkaListenerContainerFactory;
import org.springframework.kafka.core.*;
import org.springframework.kafka.support.serializer.JsonDeserializer;
import org.springframework.kafka.support.serializer.JsonSerializer;

import java.util.HashMap;
import java.util.Map;

@Configuration
@EnableKafka
public class KafkaConfig {
    
    @Bean
    public ProducerFactory<String, Object> producerFactory() {
        Map<String, Object> configProps = new HashMap<>();
        configProps.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        configProps.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class);
        configProps.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, JsonSerializer.class);
        return new DefaultKafkaProducerFactory<>(configProps);
    }
    
    @Bean
    public KafkaTemplate<String, Object> kafkaTemplate() {
        return new KafkaTemplate<>(producerFactory());
    }
    
    @Bean
    public ConsumerFactory<String, Object> consumerFactory() {
        Map<String, Object> props = new HashMap<>();
        props.put(ConsumerConfig.BOOTSTRAP_SERVERS_CONFIG, "localhost:9092");
        props.put(ConsumerConfig.GROUP_ID_CONFIG, "my-group");
        props.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class);
        props.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, JsonDeserializer.class);
        props.put(JsonDeserializer.TRUSTED_PACKAGES, "*");
        return new DefaultKafkaConsumerFactory<>(props);
    }
    
    @Bean
    public ConcurrentKafkaListenerContainerFactory<String, Object> kafkaListenerContainerFactory() {
        ConcurrentKafkaListenerContainerFactory<String, Object> factory =
            new ConcurrentKafkaListenerContainerFactory<>();
        factory.setConsumerFactory(consumerFactory());
        return factory;
    }
}

// Kafka 消息发送服务
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.kafka.support.SendResult;
import org.springframework.stereotype.Service;
import org.springframework.util.concurrent.ListenableFuture;
import org.springframework.util.concurrent.ListenableFutureCallback;

@Service
public class KafkaProducerService {
    
    private static final String TOPIC = "my-topic";
    
    @Autowired
    private KafkaTemplate<String, Object> kafkaTemplate;
    
    public void sendMessage(String message) {
        ListenableFuture<SendResult<String, Object>> future = 
            kafkaTemplate.send(TOPIC, message);
        
        future.addCallback(new ListenableFutureCallback<SendResult<String, Object>>() {
            @Override
            public void onSuccess(SendResult<String, Object> result) {
                System.out.println("Message sent successfully: " + message);
            }
            
            @Override
            public void onFailure(Throwable ex) {
                System.err.println("Failed to send message: " + ex.getMessage());
            }
        });
    }
    
    public void sendObject(Object object) {
        kafkaTemplate.send(TOPIC, object);
    }
}

// Kafka 消息监听服务
import org.springframework.kafka.annotation.KafkaListener;
import org.springframework.stereotype.Service;

@Service
public class KafkaConsumerService {
    
    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void listen(String message) {
        System.out.println("Received message: " + message);
    }
    
    @KafkaListener(topics = "my-topic", groupId = "my-group")
    public void listenObject(Object object) {
        System.out.println("Received object: " + object);
    }
}
```

### Spring Cache 配置
```java
import org.springframework.cache.annotation.EnableCaching;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.cache.RedisCacheConfiguration;
import org.springframework.data.redis.cache.RedisCacheManager;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.RedisSerializationContext;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;

@Configuration
@EnableCaching
public class CacheConfig {
    
    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofHours(1))
            .serializeKeysWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new GenericJackson2JsonRedisSerializer()))
            .disableCachingNullValues();
        
        return RedisCacheManager.builder(connectionFactory)
            .cacheDefaults(config)
            .build();
    }
}

// 缓存使用示例
import org.springframework.cache.annotation.CacheEvict;
import org.springframework.cache.annotation.CachePut;
import org.springframework.cache.annotation.Cacheable;
import org.springframework.stereotype.Service;

@Service
public class UserService {
    
    @Cacheable(value = "users", key = "#id")
    public User getUserById(Long id) {
        System.out.println("Fetching user from database: " + id);
        return new User(id, "username", "email");
    }
    
    @CachePut(value = "users", key = "#user.id")
    public User updateUser(User user) {
        System.out.println("Updating user: " + user.getId());
        return user;
    }
    
    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(Long id) {
        System.out.println("Deleting user: " + id);
    }
    
    @CacheEvict(value = "users", allEntries = true)
    public void clearAllUsers() {
        System.out.println("Clearing all users from cache");
    }
}

class User {
    private Long id;
    private String username;
    private String email;
    
    public User(Long id, String username, String email) {
        this.id = id;
        this.username = username;
        this.email = email;
    }
    
    public Long getId() { return id; }
    public String getUsername() { return username; }
    public String getEmail() { return email; }
}
```

### Spring Boot Actuator 配置
```yaml
# application.yml
management:
  endpoints:
    web:
      exposure:
        include: health,info,metrics,prometheus,loggers
      base-path: /actuator
  endpoint:
    health:
      show-details: always
      probes:
        enabled: true
  metrics:
    tags:
      application: ${spring.application.name}
    export:
      prometheus:
        enabled: true
```

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;

@Component
public class CustomHealthIndicator implements HealthIndicator {
    
    @Override
    public Health health() {
        // 自定义健康检查逻辑
        boolean healthy = checkCustomService();
        if (healthy) {
            return Health.up()
                .withDetail("customService", "Available")
                .build();
        } else {
            return Health.down()
                .withDetail("customService", "Unavailable")
                .build();
        }
    }
    
    private boolean checkCustomService() {
        return true;
    }
}

// 自定义 Info 指标
import org.springframework.boot.actuate.info.Info;
import org.springframework.boot.actuate.info.InfoContributor;
import org.springframework.stereotype.Component;

@Component
public class CustomInfoContributor implements InfoContributor {
    
    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("custom-info", "value")
            .withDetail("build-time", System.currentTimeMillis());
    }
}
```

### 多环境 Profile 配置
```yaml
# application.yml - 通用配置
spring:
  application:
    name: my-app
  profiles:
    active: dev

---
# application-dev.yml - 开发环境
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/dev_db
    username: root
    password: dev_password
  jpa:
    show-sql: true

logging:
  level:
    root: DEBUG
    com.example: DEBUG

---
# application-test.yml - 测试环境
spring:
  datasource:
    url: jdbc:mysql://test-db:3306/test_db
    username: test_user
    password: test_password

logging:
  level:
    root: INFO
    com.example: INFO

---
# application-prod.yml - 生产环境
spring:
  datasource:
    url: jdbc:mysql://prod-db:3306/prod_db
    username: ${DB_USERNAME}
    password: ${DB_PASSWORD}
  jpa:
    show-sql: false

logging:
  level:
    root: WARN
    com.example: INFO
  file:
    name: /var/log/my-app/application.log
```

### Docker 配置
```dockerfile
# Dockerfile
FROM openjdk:17-jdk-slim

WORKDIR /app

COPY target/my-app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]

# 或者使用多阶段构建
FROM maven:3.8-openjdk-17 AS build
WORKDIR /app
COPY pom.xml .
COPY src ./src
RUN mvn clean package -DskipTests

FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "app.jar"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "8080:8080"
    environment:
      - SPRING_PROFILES_ACTIVE=prod
      - DB_HOST=mysql
      - DB_PORT=3306
      - DB_NAME=mydb
      - DB_USERNAME=root
      - DB_PASSWORD=password
    depends_on:
      - mysql
      - redis
    networks:
      - app-network

  mysql:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: mydb
    ports:
      - "3306:3306"
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - app-network

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data
    networks:
      - app-network

volumes:
  mysql-data:
  redis-data:

networks:
  app-network:
    driver: bridge
```

### Kubernetes 部署配置
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-app
        image: my-app:latest
        ports:
        - containerPort: 8080
        env:
        - name: SPRING_PROFILES_ACTIVE
          value: "prod"
        - name: DB_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: db.host
        - name: DB_PASSWORD
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: db.password
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
        livenessProbe:
          httpGet:
            path: /actuator/health/liveness
            port: 8080
          initialDelaySeconds: 60
          periodSeconds: 10
        readinessProbe:
          httpGet:
            path: /actuator/health/readiness
            port: 8080
          initialDelaySeconds: 30
          periodSeconds: 5

---
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  selector:
    app: my-app
  ports:
  - protocol: TCP
    port: 80
    targetPort: 8080
  type: LoadBalancer

---
# configmap.yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  db.host: "mysql-service"
  db.port: "3306"
  db.name: "mydb"

---
# secret.yaml
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
type: Opaque
data:
  db.username: cm9vdA==
  db.password: cGFzc3dvcmQ=
```

## 4. 官方示例代码（可运行）

### 完整的消息队列示例
```java
// 订单消息实体
public class OrderMessage {
    private String orderId;
    private String productId;
    private Integer quantity;
    private Double amount;

    // getters and setters
    public String getOrderId() { return orderId; }
    public void setOrderId(String orderId) { this.orderId = orderId; }
    public String getProductId() { return productId; }
    public void setProductId(String productId) { this.productId = productId; }
    public Integer getQuantity() { return quantity; }
    public void setQuantity(Integer quantity) { this.quantity = quantity; }
    public Double getAmount() { return amount; }
    public void setAmount(Double amount) { this.amount = amount; }
}

// 订单服务
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class OrderService {
    
    @Autowired
    private MessageProducer messageProducer;
    
    public void createOrder(OrderMessage order) {
        System.out.println("Creating order: " + order.getOrderId());
        messageProducer.sendObject(order);
    }
}

// 订单消息消费者
import org.springframework.amqp.rabbit.annotation.RabbitListener;
import org.springframework.stereotype.Service;

@Service
public class OrderConsumer {
    
    @RabbitListener(queues = RabbitMQConfig.QUEUE_NAME)
    public void handleOrder(OrderMessage order) {
        System.out.println("Processing order: " + order.getOrderId());
        processOrder(order);
    }
    
    private void processOrder(OrderMessage order) {
        System.out.println("Order processed: " + order.getOrderId());
    }
}

// 订单控制器
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

@RestController
@RequestMapping("/api/orders")
public class OrderController {
    
    @Autowired
    private OrderService orderService;
    
    @PostMapping
    public ResponseEntity<String> createOrder(@RequestBody OrderMessage order) {
        order.setOrderId("ORD-" + System.currentTimeMillis());
        orderService.createOrder(order);
        return ResponseEntity.ok("Order created: " + order.getOrderId());
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 消息队列最佳实践
1. **消息持久化**：重要消息设置持久化
2. **消息确认**：使用手动确认确保消息不丢失
3. **幂等性**：消费者确保消息处理的幂等性
4. **死信队列**：配置死信队列处理失败消息
5. **监控告警**：监控队列堆积和消费延迟
6. **限流**：根据系统能力限制消费速度

### 缓存最佳实践
1. **合理设置 TTL**：根据数据更新频率设置过期时间
2. **缓存穿透**：使用布隆过滤器防止缓存穿透
3. **缓存击穿**：热点数据使用互斥锁或永不过期
4. **缓存雪崩**：错开缓存过期时间
5. **缓存更新**：先更新数据库，再删除缓存
6. **缓存预热**：系统启动时加载热点数据

### 配置管理最佳实践
1. **敏感信息加密**：使用 Jasypt 等工具加密配置
2. **外部化配置**：不要将配置硬编码在代码中
3. **Profile 隔离**：不同环境使用不同的 Profile
4. **配置验证**：启动时验证必需配置
5. **配置热更新**：使用 Spring Cloud Config 等实现热更新

### 部署最佳实践
1. **容器化**：使用 Docker 容器化应用
2. **健康检查**：配置健康检查探针
3. **资源限制**：设置合理的 CPU 和内存限制
4. **日志管理**：集中化日志收集和分析
5. **监控告警**：配置应用监控和告警
6. **蓝绿部署**：使用蓝绿部署减少停机时间
7. **回滚策略**：准备快速回滚方案

### 性能优化
1. **JVM 参数调优**：根据应用特性调整 JVM 参数
2. **连接池配置**：合理配置数据库和连接池大小
3. **异步处理**：耗时操作使用异步处理
4. **压缩**：启用 HTTP 响应压缩
5. **静态资源缓存**：配置静态资源缓存策略

## 6. 官方文档参考链接

- Spring Boot 官方文档：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/
- Spring AMQP (RabbitMQ)：https://docs.spring.io/spring-amqp/reference/
- Spring Kafka：https://docs.spring.io/spring-kafka/reference/
- Spring Cache：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#io.caching
- Spring Boot Actuator：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator
- Docker 官方文档：https://docs.docker.com/
- Kubernetes 官方文档：https://kubernetes.io/docs/
- Spring Boot Docker：https://spring.io/guides/gs/spring-boot-docker/
- Spring Boot Kubernetes：https://spring.io/guides/gs/spring-boot-kubernetes/
