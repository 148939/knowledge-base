# 检索索引：本文档收录【Spring Cloud】【微服务】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Spring Cloud 是一系列框架的有序集合，它利用 Spring Boot 的开发便利性简化了分布式系统基础设施的开发，如服务发现注册、配置中心、消息总线、负载均衡、熔断器、数据监控等。Spring Cloud 为开发者提供了在分布式系统（配置管理、服务发现、断路器、智能路由、微代理、控制总线、全局锁、决策竞选、分布式会话和集群状态）中快速构建一些常见模式的工具。

## 2. 核心知识点（结构化层级）

### 2.1 核心组件
1. 服务注册与发现
   - Eureka
   - Nacos
   - Consul
2. 服务调用
   - Ribbon
   - OpenFeign
3. 服务熔断与降级
   - Hystrix
   - Sentinel
   - Resilience4j
4. 服务网关
   - Spring Cloud Gateway
   - Zuul
5. 配置中心
   - Spring Cloud Config
   - Nacos Config
6. 消息驱动
   - Spring Cloud Stream
7. 分布式事务
   - Seata
8. 链路追踪
   - Sleuth
   - Zipkin

### 2.2 服务注册与发现
1. Eureka Server 搭建
2. Eureka Client 集成
3. 服务注册
4. 服务发现
5. 服务健康检查
6. Eureka 高可用

### 2.3 服务调用
1. OpenFeign 声明式调用
2. Ribbon 负载均衡
3. 自定义负载均衡策略
4. 服务间调用

### 2.4 服务熔断与限流
1. Sentinel 集成
2. 熔断降级规则
3. 流量控制规则
4. 热点参数限流
5. 系统自适应保护

### 2.5 服务网关
1. Spring Cloud Gateway 搭建
2. 路由配置
3. 断言工厂
4. 过滤器工厂
5. 全局过滤器
6. 限流配置
7. 跨域处理

### 2.6 配置中心
1. Nacos Config 使用
2. 配置管理
3. 配置动态刷新
4. 配置版本管理
5. 多环境配置

## 3. 官方标准语法 / API

### 项目依赖配置
```xml
<!-- pom.xml -->
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>2023.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
        <dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-dependencies</artifactId>
            <version>2023.0.1.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>

<!-- Nacos 服务发现 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
</dependency>

<!-- Nacos 配置中心 -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-nacos-config</artifactId>
</dependency>

<!-- OpenFeign -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-openfeign</artifactId>
</dependency>

<!-- Spring Cloud Gateway -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>

<!-- Sentinel -->
<dependency>
    <groupId>com.alibaba.cloud</groupId>
    <artifactId>spring-cloud-starter-alibaba-sentinel</artifactId>
</dependency>

<!-- LoadBalancer -->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-loadbalancer</artifactId>
</dependency>
```

### Nacos 服务注册与发现
```yaml
# bootstrap.yml
spring:
  application:
    name: user-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
        namespace: public
        group: DEFAULT_GROUP
        weight: 1
        metadata:
          version: 1.0
          env: dev
      config:
        server-addr: localhost:8848
        namespace: public
        group: DEFAULT_GROUP
        file-extension: yml
        shared-configs:
          - data-id: common.yml
            group: DEFAULT_GROUP
            refresh: true
```

```java
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}
```

### OpenFeign 服务调用
```java
// 启动类
@SpringBootApplication
@EnableFeignClients
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

// Feign 客户端接口
@FeignClient(name = "user-service", path = "/users")
public interface UserFeignClient {
    
    @GetMapping("/{id}")
    UserDTO getUserById(@PathVariable("id") Long id);
    
    @GetMapping
    List<UserDTO> getUserList();
    
    @PostMapping
    UserDTO createUser(@RequestBody UserDTO user);
    
    @PutMapping("/{id}")
    UserDTO updateUser(@PathVariable("id") Long id, @RequestBody UserDTO user);
    
    @DeleteMapping("/{id}")
    void deleteUser(@PathVariable("id") Long id);
}

// Feign 配置
@Configuration
public class FeignConfig {
    
    @Bean
    Logger.Level feignLoggerLevel() {
        return Logger.Level.FULL;
    }
    
    @Bean
    public Request.Options options() {
        return new Request.Options(5, TimeUnit.SECONDS, 10, TimeUnit.SECONDS, true);
    }
    
    @Bean
    public Retryer retryer() {
        return new Retryer.Default(100, 1000, 3);
    }
}

// 使用 Feign 客户端
@Service
public class OrderService {
    
    @Autowired
    private UserFeignClient userFeignClient;
    
    public OrderDTO createOrder(OrderCreateDTO orderCreateDTO) {
        // 调用用户服务
        UserDTO user = userFeignClient.getUserById(orderCreateDTO.getUserId());
        
        if (user == null) {
            throw new BusinessException("用户不存在");
        }
        
        // 创建订单逻辑
        // ...
        
        return orderDTO;
    }
}
```

### Spring Cloud Gateway 网关
```yaml
spring:
  application:
    name: gateway-service
  cloud:
    nacos:
      discovery:
        server-addr: localhost:8848
    gateway:
      discovery:
        locator:
          enabled: true
          lower-case-service-id: true
      routes:
        - id: user-service
          uri: lb://user-service
          predicates:
            - Path=/users/**
          filters:
            - StripPrefix=0
            - AddRequestHeader=X-Gateway-Version, 1.0
            
        - id: order-service
          uri: lb://order-service
          predicates:
            - Path=/orders/**
            - Method=GET,POST
          filters:
            - StripPrefix=0
            - name: RequestRateLimiter
              args:
                redis-rate-limiter.replenishRate: 10
                redis-rate-limiter.burstCapacity: 20
                
        - id: product-service
          uri: lb://product-service
          predicates:
            - Path=/products/**
            - Before=2025-12-31T23:59:59+08:00[Asia/Shanghai]
          filters:
            - StripPrefix=0
```

```java
@SpringBootApplication
@EnableDiscoveryClient
public class GatewayServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(GatewayServiceApplication.class, args);
    }
}

// 全局过滤器
@Component
public class GlobalLoggingFilter implements GlobalFilter, Ordered {
    
    private static final Logger log = LoggerFactory.getLogger(GlobalLoggingFilter.class);
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        String path = request.getURI().getPath();
        String method = request.getMethod().name();
        
        log.info("Request: {} {}", method, path);
        
        long startTime = System.currentTimeMillis();
        
        return chain.filter(exchange).then(Mono.fromRunnable(() -> {
            long endTime = System.currentTimeMillis();
            log.info("Response: {} {} took {}ms", method, path, (endTime - startTime));
        }));
    }
    
    @Override
    public int getOrder() {
        return -1;
    }
}

// 认证过滤器
@Component
public class AuthenticationFilter implements GlobalFilter, Ordered {
    
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest();
        String token = request.getHeaders().getFirst("Authorization");
        
        // 白名单路径
        String path = request.getURI().getPath();
        if (path.startsWith("/auth/") || path.startsWith("/public/")) {
            return chain.filter(exchange);
        }
        
        if (token == null || token.isEmpty()) {
            ServerHttpResponse response = exchange.getResponse();
            response.setStatusCode(HttpStatus.UNAUTHORIZED);
            response.getHeaders().setContentType(MediaType.APPLICATION_JSON);
            String body = "{\"code\": 401, \"message\": \"未授权\"}";
            DataBuffer buffer = response.bufferFactory().wrap(body.getBytes());
            return response.writeWith(Mono.just(buffer));
        }
        
        // 验证 token
        // ...
        
        return chain.filter(exchange);
    }
    
    @Override
    public int getOrder() {
        return 0;
    }
}

// 自定义断言工厂
@Component
public class CheckAuthRoutePredicateFactory 
        extends AbstractRoutePredicateFactory<CheckAuthRoutePredicateFactory.Config> {
    
    public CheckAuthRoutePredicateFactory() {
        super(Config.class);
    }
    
    @Override
    public Predicate<ServerWebExchange> apply(Config config) {
        return exchange -> {
            ServerHttpRequest request = exchange.getRequest();
            String path = request.getURI().getPath();
            
            // 检查是否需要认证
            if (config.isRequireAuth() && !isPublicPath(path)) {
                String token = request.getHeaders().getFirst("Authorization");
                return token != null && validateToken(token);
            }
            
            return true;
        };
    }
    
    private boolean isPublicPath(String path) {
        return path.startsWith("/public/") || path.startsWith("/auth/");
    }
    
    private boolean validateToken(String token) {
        // 验证 token 逻辑
        return true;
    }
    
    public static class Config {
        private boolean requireAuth = true;
        
        public boolean isRequireAuth() {
            return requireAuth;
        }
        
        public void setRequireAuth(boolean requireAuth) {
            this.requireAuth = requireAuth;
        }
    }
}
```

### Sentinel 熔断限流
```yaml
spring:
  cloud:
    sentinel:
      transport:
        dashboard: localhost:8080
        port: 8719
      eager: true
      datasource:
        flow:
          nacos:
            server-addr: localhost:8848
            dataId: ${spring.application.name}-flow-rules
            groupId: SENTINEL_GROUP
            rule-type: flow
        degrade:
          nacos:
            server-addr: localhost:8848
            dataId: ${spring.application.name}-degrade-rules
            groupId: SENTINEL_GROUP
            rule-type: degrade
```

```java
// 资源定义
@Service
public class UserService {
    
    @SentinelResource(value = "getUser", blockHandler = "handleBlock", fallback = "handleFallback")
    public UserDTO getUser(Long id) {
        // 业务逻辑
        return userMapper.selectById(id);
    }
    
    // 限流/熔断处理
    public UserDTO handleBlock(Long id, BlockException exception) {
        return new UserDTO();
    }
    
    // 异常降级处理
    public UserDTO handleFallback(Long id, Throwable throwable) {
        return new UserDTO();
    }
}

// 代码方式配置规则
@Configuration
public class SentinelConfig {
    
    @PostConstruct
    public void initFlowRules() {
        List<FlowRule> rules = new ArrayList<>();
        
        FlowRule rule = new FlowRule();
        rule.setResource("getUser");
        rule.setGrade(RuleConstant.FLOW_GRADE_QPS);
        rule.setCount(10);
        rule.setLimitApp("default");
        rule.setControlBehavior(RuleConstant.CONTROL_BEHAVIOR_DEFAULT);
        
        rules.add(rule);
        FlowRuleManager.loadRules(rules);
    }
    
    @PostConstruct
    public void initDegradeRules() {
        List<DegradeRule> rules = new ArrayList<>();
        
        DegradeRule rule = new DegradeRule();
        rule.setResource("getUser");
        rule.setGrade(RuleConstant.DEGRADE_GRADE_RT);
        rule.setCount(1000);
        rule.setTimeWindow(10);
        rule.setMinRequestAmount(5);
        rule.setStatIntervalMs(10000);
        
        rules.add(rule);
        DegradeRuleManager.loadRules(rules);
    }
}
```

### 配置中心使用
```yaml
# bootstrap.yml
spring:
  application:
    name: user-service
  cloud:
    nacos:
      config:
        server-addr: localhost:8848
        namespace: public
        group: DEFAULT_GROUP
        file-extension: yml
        refresh-enabled: true
        shared-configs:
          - data-id: common.yml
            group: DEFAULT_GROUP
            refresh: true
        extension-configs:
          - data-id: database.yml
            group: DEFAULT_GROUP
            refresh: true
```

```java
@RestController
@RequestMapping("/config")
@RefreshScope
public class ConfigController {
    
    @Value("${app.name:default}")
    private String appName;
    
    @Value("${app.version:1.0}")
    private String appVersion;
    
    @Autowired
    private AppConfig appConfig;
    
    @GetMapping("/info")
    public Map<String, Object> getConfigInfo() {
        Map<String, Object> result = new HashMap<>();
        result.put("appName", appName);
        result.put("appVersion", appVersion);
        result.put("config", appConfig);
        return result;
    }
}

@Data
@Component
@ConfigurationProperties(prefix = "app")
@RefreshScope
public class AppConfig {
    private String name;
    private String version;
    private String environment;
    private List<String> features;
}
```

## 4. 官方示例代码（可运行）

### 微服务完整项目结构
```
microservice-demo/
├── gateway-service/          # 网关服务
├── user-service/             # 用户服务
├── order-service/            # 订单服务
├── product-service/          # 商品服务
└── common/                   # 公共模块
```

### 公共模块
```java
// common/src/main/java/com/example/common/dto/UserDTO.java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class UserDTO {
    private Long id;
    private String username;
    private String email;
    private Integer age;
    private Integer status;
}

// common/src/main/java/com/example/common/dto/OrderDTO.java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class OrderDTO {
    private Long id;
    private Long userId;
    private Long productId;
    private Integer quantity;
    private BigDecimal totalAmount;
    private String status;
    private LocalDateTime createTime;
}

// common/src/main/java/com/example/common/result/Result.java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class Result<T> {
    private Integer code;
    private String message;
    private T data;
    
    public static <T> Result<T> success(T data) {
        return new Result<>(200, "success", data);
    }
    
    public static <T> Result<T> error(String message) {
        return new Result<>(500, message, null);
    }
    
    public static <T> Result<T> error(Integer code, String message) {
        return new Result<>(code, message, null);
    }
}

// common/src/main/java/com/example/common/exception/BusinessException.java
@Getter
public class BusinessException extends RuntimeException {
    private final Integer code;
    
    public BusinessException(String message) {
        super(message);
        this.code = 500;
    }
    
    public BusinessException(Integer code, String message) {
        super(message);
        this.code = code;
    }
}

// common/src/main/java/com/example/common/exception/GlobalExceptionHandler.java
@RestControllerAdvice
public class GlobalExceptionHandler {
    
    @ExceptionHandler(BusinessException.class)
    public Result<Void> handleBusinessException(BusinessException e) {
        return Result.error(e.getCode(), e.getMessage());
    }
    
    @ExceptionHandler(Exception.class)
    public Result<Void> handleException(Exception e) {
        log.error("系统异常", e);
        return Result.error("系统异常");
    }
}
```

### 用户服务
```java
@SpringBootApplication
@EnableDiscoveryClient
public class UserServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserServiceApplication.class, args);
    }
}

@RestController
@RequestMapping("/users")
public class UserController {
    
    @Autowired
    private UserService userService;
    
    @GetMapping("/{id}")
    public Result<UserDTO> getById(@PathVariable Long id) {
        UserDTO user = userService.getById(id);
        return Result.success(user);
    }
    
    @GetMapping
    public Result<List<UserDTO>> list() {
        List<UserDTO> users = userService.list();
        return Result.success(users);
    }
    
    @PostMapping
    public Result<UserDTO> create(@RequestBody UserDTO userDTO) {
        UserDTO user = userService.create(userDTO);
        return Result.success(user);
    }
    
    @PutMapping("/{id}")
    public Result<UserDTO> update(@PathVariable Long id, @RequestBody UserDTO userDTO) {
        userDTO.setId(id);
        UserDTO user = userService.update(userDTO);
        return Result.success(user);
    }
    
    @DeleteMapping("/{id}")
    public Result<Void> delete(@PathVariable Long id) {
        userService.delete(id);
        return Result.success();
    }
}

@Service
public class UserService {
    
    @Autowired
    private UserMapper userMapper;
    
    @SentinelResource(value = "getUser", blockHandler = "handleBlock")
    public UserDTO getById(Long id) {
        User user = userMapper.selectById(id);
        if (user == null) {
            throw new BusinessException("用户不存在");
        }
        return convertToDTO(user);
    }
    
    public List<UserDTO> list() {
        List<User> users = userMapper.selectList(null);
        return users.stream()
                .map(this::convertToDTO)
                .collect(Collectors.toList());
    }
    
    @Transactional(rollbackFor = Exception.class)
    public UserDTO create(UserDTO userDTO) {
        // 检查用户名是否存在
        LambdaQueryWrapper<User> wrapper = new LambdaQueryWrapper<>();
        wrapper.eq(User::getUsername, userDTO.getUsername());
        User existUser = userMapper.selectOne(wrapper);
        
        if (existUser != null) {
            throw new BusinessException("用户名已存在");
        }
        
        User user = new User();
        BeanUtils.copyProperties(userDTO, user);
        user.setStatus(1);
        userMapper.insert(user);
        
        return convertToDTO(user);
    }
    
    @Transactional(rollbackFor = Exception.class)
    public UserDTO update(UserDTO userDTO) {
        User user = userMapper.selectById(userDTO.getId());
        if (user == null) {
            throw new BusinessException("用户不存在");
        }
        
        BeanUtils.copyProperties(userDTO, user, "id", "createTime");
        userMapper.updateById(user);
        
        return convertToDTO(user);
    }
    
    @Transactional(rollbackFor = Exception.class)
    public void delete(Long id) {
        User user = userMapper.selectById(id);
        if (user == null) {
            throw new BusinessException("用户不存在");
        }
        
        userMapper.deleteById(id);
    }
    
    public UserDTO handleBlock(Long id, BlockException exception) {
        throw new BusinessException("系统繁忙，请稍后再试");
    }
    
    private UserDTO convertToDTO(User user) {
        UserDTO dto = new UserDTO();
        BeanUtils.copyProperties(user, dto);
        return dto;
    }
}
```

### 订单服务（调用用户服务）
```java
@SpringBootApplication
@EnableDiscoveryClient
@EnableFeignClients
public class OrderServiceApplication {
    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
}

@FeignClient(name = "user-service", path = "/users")
public interface UserFeignClient {
    
    @GetMapping("/{id}")
    Result<UserDTO> getUserById(@PathVariable("id") Long id);
    
    @GetMapping
    Result<List<UserDTO>> getUserList();
}

@RestController
@RequestMapping("/orders")
public class OrderController {
    
    @Autowired
    private OrderService orderService;
    
    @GetMapping("/{id}")
    public Result<OrderDTO> getById(@PathVariable Long id) {
        OrderDTO order = orderService.getById(id);
        return Result.success(order);
    }
    
    @PostMapping
    public Result<OrderDTO> create(@RequestBody OrderCreateDTO orderCreateDTO) {
        OrderDTO order = orderService.create(orderCreateDTO);
        return Result.success(order);
    }
}

@Service
public class OrderService {
    
    @Autowired
    private OrderMapper orderMapper;
    
    @Autowired
    private UserFeignClient userFeignClient;
    
    @SentinelResource(value = "createOrder", fallback = "handleFallback")
    @Transactional(rollbackFor = Exception.class)
    public OrderDTO create(OrderCreateDTO orderCreateDTO) {
        // 调用用户服务验证用户
        Result<UserDTO> userResult = userFeignClient.getUserById(orderCreateDTO.getUserId());
        
        if (userResult.getCode() != 200 || userResult.getData() == null) {
            throw new BusinessException("用户不存在");
        }
        
        UserDTO user = userResult.getData();
        
        // 创建订单
        Order order = new Order();
        order.setUserId(orderCreateDTO.getUserId());
        order.setProductId(orderCreateDTO.getProductId());
        order.setQuantity(orderCreateDTO.getQuantity());
        order.setTotalAmount(orderCreateDTO.getTotalAmount());
        order.setStatus("PENDING");
        orderMapper.insert(order);
        
        return convertToDTO(order);
    }
    
    public OrderDTO getById(Long id) {
        Order order = orderMapper.selectById(id);
        if (order == null) {
            throw new BusinessException("订单不存在");
        }
        
        OrderDTO dto = convertToDTO(order);
        
        // 调用用户服务获取用户信息
        Result<UserDTO> userResult = userFeignClient.getUserById(order.getUserId());
        if (userResult.getCode() == 200) {
            dto.setUser(userResult.getData());
        }
        
        return dto;
    }
    
    public OrderDTO handleFallback(OrderCreateDTO orderCreateDTO, Throwable throwable) {
        throw new BusinessException("订单创建失败，请稍后再试");
    }
    
    private OrderDTO convertToDTO(Order order) {
        OrderDTO dto = new OrderDTO();
        BeanUtils.copyProperties(order, dto);
        return dto;
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 服务拆分原则
1. **单一职责**：每个微服务只负责一个特定的业务领域
2. **独立部署**：每个微服务可以独立部署和扩展
3. **数据隔离**：每个微服务拥有自己的数据库
4. **API 设计**：RESTful API 设计规范，版本管理
5. **容错设计**：服务间调用考虑熔断、降级、限流

### 服务注册与发现注意事项
1. **健康检查**：配置合理的健康检查机制
2. **服务元数据**：合理使用元数据存储服务信息
3. **多环境隔离**：不同环境使用不同的 namespace
4. **权重配置**：根据服务器性能配置权重
5. **优雅上下线**：确保服务上下线时流量平滑过渡

### 服务调用最佳实践
1. **超时配置**：合理设置超时时间，避免长时间阻塞
2. **重试策略**：配置合适的重试策略，注意幂等性
3. **负载均衡**：选择合适的负载均衡策略
4. **连接池**：配置合适的连接池参数
5. **日志记录**：记录关键调用日志便于问题排查

### 熔断限流注意事项
1. **资源命名**：合理命名资源便于管理
2. **规则配置**：根据实际情况调整限流阈值
3. **降级策略**：提供友好的降级方案
4. **监控告警**：配置监控和告警及时发现问题
5. **规则持久化**：将规则持久化到配置中心

### 网关最佳实践
1. **路由设计**：合理设计路由规则，便于维护
2. **统一认证**：在网关层统一处理认证授权
3. **日志监控**：记录请求日志，便于问题追踪
4. **性能优化**：配置合理的缓存和压缩
5. **安全防护**：配置 WAF、限流等安全措施

### 配置中心注意事项
1. **配置分类**：合理分类配置，公共配置共享
2. **环境隔离**：不同环境配置隔离
3. **版本管理**：配置变更记录版本，支持回滚
4. **敏感信息**：敏感配置加密存储
5. **动态刷新**：配置变更后及时刷新

### 最佳实践总结
1. **服务拆分**：合理拆分微服务，避免过度拆分
2. **API 网关**：统一入口，处理横切关注点
3. **服务治理**：注册发现、配置管理、熔断限流
4. **链路追踪**：实现全链路追踪，便于问题排查
5. **监控告警**：完善监控体系，及时发现问题
6. **日志管理**：统一日志格式，集中日志管理
7. **容器化部署**：使用 Docker 和 K8s 部署
8. **CI/CD**：实现自动化测试和部署
9. **文档完善**：保持 API 文档和架构文档更新
10. **故障演练**：定期进行故障演练，提升应急能力

## 6. 官方文档参考链接

- Spring Cloud 官方文档：https://spring.io/projects/spring-cloud
- Spring Cloud Alibaba 官方文档：https://github.com/alibaba/spring-cloud-alibaba
- Nacos 官方文档：https://nacos.io/zh-cn/
- Sentinel 官方文档：https://sentinelguard.io/zh-cn/
- Spring Cloud Gateway 官方文档：https://docs.spring.io/spring-cloud-gateway/
