# 检索索引：本文档收录【Spring Boot】【核心自动配置】知识点，内容100%来自Spring官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Spring Boot 的核心特性是自动配置（Auto-configuration），它可以根据你添加的 jar 依赖自动配置 Spring 应用。自动配置尝试根据类路径内容和你定义的 bean 来猜测和配置你需要的 bean。Spring Boot 通过 @EnableAutoConfiguration 或 @SpringBootApplication 注解来启用自动配置。

## 2. 核心知识点（结构化层级）

### 2.1 自动配置基础
1. @SpringBootApplication 注解
2. @EnableAutoConfiguration 注解
3. 自动配置的工作原理
4. 条件化配置（@Conditional）
5. 自动配置类

### 2.2 条件注解
1. @ConditionalOnClass - 类路径下存在指定类
2. @ConditionalOnMissingClass - 类路径下不存在指定类
3. @ConditionalOnBean - 容器中存在指定 bean
4. @ConditionalOnMissingBean - 容器中不存在指定 bean
5. @ConditionalOnProperty - 配置文件中存在指定属性
6. @ConditionalOnResource - 存在指定资源
7. @ConditionalOnWebApplication - 是 Web 应用
8. @ConditionalOnNotWebApplication - 不是 Web 应用
9. @ConditionalOnExpression - SpEL 表达式为 true

### 2.3 自动配置属性
1. @ConfigurationProperties 注解
2. @EnableConfigurationProperties 注解
3. 外部化配置
4. 配置属性绑定
5. 类型安全的配置

### 2.4 自定义自动配置
1. 创建自动配置类
2. 注册自动配置
3. 自定义 starter
4. 配置元数据
5. 自动配置报告

### 2.5 禁用自动配置
1. 排除特定自动配置类
2. 使用属性禁用
3. 自动配置的优先级

## 3. 官方标准语法 / API

### @SpringBootApplication 注解
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

// @SpringBootApplication 包含以下三个注解：
// 1. @Configuration - 标记为配置类
// 2. @EnableAutoConfiguration - 启用自动配置
// 3. @ComponentScan - 扫描组件
@SpringBootApplication
public class MyApplication {

    public static void main(String[] args) {
        SpringApplication.run(MyApplication.class, args);
    }

}

// 等效的完整写法
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;

@Configuration
@EnableAutoConfiguration
@ComponentScan
public class MyApplication2 {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication2.class, args);
    }
}

// 排除特定的自动配置类
@SpringBootApplication(exclude = { DataSourceAutoConfiguration.class })
public class MyApplication3 {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication3.class, args);
    }
}

// 使用 excludeName 排除（通过类名）
@SpringBootApplication(excludeName = "org.springframework.boot.autoconfigure.jdbc.DataSourceAutoConfiguration")
public class MyApplication4 {
    public static void main(String[] args) {
        SpringApplication.run(MyApplication4.class, args);
    }
}
```

### @EnableAutoConfiguration 注解
```java
import org.springframework.boot.autoconfigure.EnableAutoConfiguration;
import org.springframework.context.annotation.Configuration;

// 启用自动配置
@EnableAutoConfiguration
@Configuration
public class AppConfig {
}

// 排除自动配置
@EnableAutoConfiguration(exclude = { DataSourceAutoConfiguration.class })
@Configuration
public class AppConfig2 {
}

// 自定义自动配置导入
import org.springframework.context.annotation.Import;
import org.springframework.boot.autoconfigure.AutoConfigurationImportSelector;
import org.springframework.context.annotation.Configuration;

@Configuration
@Import(AutoConfigurationImportSelector.class)
public class CustomAutoConfig {
}
```

### 条件注解示例
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.boot.autoconfigure.condition.*;

@Configuration
public class MyAutoConfiguration {

    // 当类路径下存在 RedisTemplate 时创建 bean
    @Bean
    @ConditionalOnClass(RedisTemplate.class)
    public RedisService redisService() {
        return new RedisService();
    }

    // 当容器中不存在 MyService 时创建
    @Bean
    @ConditionalOnMissingBean
    public MyService myService() {
        return new MyService();
    }

    // 当容器中不存在指定名称的 bean 时创建
    @Bean("customService")
    @ConditionalOnMissingBean(name = "customService")
    public CustomService customService() {
        return new CustomService();
    }

    // 当属性存在且有指定值时创建
    @Bean
    @ConditionalOnProperty(prefix = "myapp.feature", name = "enabled", havingValue = "true", matchIfMissing = false)
    public FeatureService featureService() {
        return new FeatureService();
    }

    // 当属性不存在时也创建（matchIfMissing = true）
    @Bean
    @ConditionalOnProperty(prefix = "myapp.cache", name = "enabled", havingValue = "true", matchIfMissing = true)
    public CacheService cacheService() {
        return new CacheService();
    }

    // 存在指定资源时创建
    @Bean
    @ConditionalOnResource(resources = "classpath:custom-config.properties")
    public ResourceService resourceService() {
        return new ResourceService();
    }

    // 是 Web 应用时创建
    @Bean
    @ConditionalOnWebApplication
    public WebService webService() {
        return new WebService();
    }

    // 不是 Web 应用时创建
    @Bean
    @ConditionalOnNotWebApplication
    public BatchService batchService() {
        return new BatchService();
    }

    // 多个条件组合
    @Bean
    @ConditionalOnClass(name = "org.springframework.data.redis.core.RedisTemplate")
    @ConditionalOnMissingBean
    @ConditionalOnProperty(prefix = "myapp.redis", name = "enabled", havingValue = "true")
    public RedisOperations redisOperations() {
        return new RedisOperations();
    }

    // SpEL 表达式条件
    @Bean
    @ConditionalOnExpression("#{systemProperties['os.name'].toLowerCase().contains('mac')}")
    public MacSpecificService macSpecificService() {
        return new MacSpecificService();
    }
}
```

### @ConfigurationProperties 注解
```java
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.stereotype.Component;
import java.util.List;
import java.util.Map;

// 简单的配置属性类
@Component
@ConfigurationProperties(prefix = "myapp")
public class MyAppProperties {
    
    private String name = "MyApp";
    private String version = "1.0.0";
    private boolean enabled = true;
    
    // getter 和 setter
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getVersion() { return version; }
    public void setVersion(String version) { this.version = version; }
    public boolean isEnabled() { return enabled; }
    public void setEnabled(boolean enabled) { this.enabled = enabled; }
}

// 嵌套配置属性
@Component
@ConfigurationProperties(prefix = "myapp.database")
public class DatabaseProperties {
    
    private String url;
    private String username;
    private String password;
    private int maxConnections = 10;
    private Pool pool = new Pool();
    
    // 嵌套类
    public static class Pool {
        private int maxIdle = 5;
        private int minIdle = 1;
        private int maxTotal = 20;
        
        // getter 和 setter
        public int getMaxIdle() { return maxIdle; }
        public void setMaxIdle(int maxIdle) { this.maxIdle = maxIdle; }
        public int getMinIdle() { return minIdle; }
        public void setMinIdle(int minIdle) { this.minIdle = minIdle; }
        public int getMaxTotal() { return maxTotal; }
        public void setMaxTotal(int maxTotal) { this.maxTotal = maxTotal; }
    }
    
    // getter 和 setter
    public String getUrl() { return url; }
    public void setUrl(String url) { this.url = url; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
    public int getMaxConnections() { return maxConnections; }
    public void setMaxConnections(int maxConnections) { this.maxConnections = maxConnections; }
    public Pool getPool() { return pool; }
    public void setPool(Pool pool) { this.pool = pool; }
}

// 列表和 Map 配置
@Component
@ConfigurationProperties(prefix = "myapp.server")
public class ServerProperties {
    
    private List<String> hosts;
    private Map<String, Integer> ports;
    private List<Feature> features;
    
    public static class Feature {
        private String name;
        private boolean enabled;
        
        // getter 和 setter
        public String getName() { return name; }
        public void setName(String name) { this.name = name; }
        public boolean isEnabled() { return enabled; }
        public void setEnabled(boolean enabled) { this.enabled = enabled; }
    }
    
    // getter 和 setter
    public List<String> getHosts() { return hosts; }
    public void setHosts(List<String> hosts) { this.hosts = hosts; }
    public Map<String, Integer> getPorts() { return ports; }
    public void setPorts(Map<String, Integer> ports) { this.ports = ports; }
    public List<Feature> getFeatures() { return features; }
    public void setFeatures(List<Feature> features) { this.features = features; }
}

// 使用 @EnableConfigurationProperties（不使用 @Component）
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties({ MyAppProperties.class, DatabaseProperties.class })
public class PropertiesConfig {
}

// 使用构造函数绑定（Spring Boot 2.2+）
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.boot.context.properties.bind.DefaultValue;

@ConfigurationProperties(prefix = "myapp.mail")
public class MailProperties {
    
    private final String host;
    private final int port;
    private final String from;
    
    public MailProperties(
            @DefaultValue("localhost") String host,
            @DefaultValue("25") int port,
            String from) {
        this.host = host;
        this.port = port;
        this.from = from;
    }
    
    // getter 方法
    public String getHost() { return host; }
    public int getPort() { return port; }
    public String getFrom() { return from; }
}

// 使用配置属性
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

@Service
public class MyService {
    
    private final MyAppProperties appProperties;
    private final DatabaseProperties dbProperties;
    
    @Autowired
    public MyService(MyAppProperties appProperties, DatabaseProperties dbProperties) {
        this.appProperties = appProperties;
        this.dbProperties = dbProperties;
    }
    
    public void printConfig() {
        System.out.println("App Name: " + appProperties.getName());
        System.out.println("Database URL: " + dbProperties.getUrl());
        System.out.println("Max Connections: " + dbProperties.getMaxConnections());
        System.out.println("Pool Max Total: " + dbProperties.getPool().getMaxTotal());
    }
}
```

### application.properties / application.yml 配置示例
```properties
# application.properties
myapp.name=My Spring Boot App
myapp.version=2.0.0
myapp.enabled=true

# 数据库配置
myapp.database.url=jdbc:mysql://localhost:3306/mydb
myapp.database.username=root
myapp.database.password=secret
myapp.database.max-connections=20
myapp.database.pool.max-idle=10
myapp.database.pool.min-idle=5
myapp.database.pool.max-total=50

# 服务器配置
myapp.server.hosts[0]=host1.example.com
myapp.server.hosts[1]=host2.example.com
myapp.server.ports.http=8080
myapp.server.ports.https=8443
myapp.server.features[0].name=cache
myapp.server.features[0].enabled=true
myapp.server.features[1].name=metrics
myapp.server.features[1].enabled=true

# 功能开关
myapp.feature.enabled=true
myapp.cache.enabled=true
myapp.redis.enabled=true

# Spring Boot 核心配置
spring.application.name=my-application
server.port=8080
```

```yaml
# application.yml
myapp:
  name: My Spring Boot App
  version: 2.0.0
  enabled: true

  # 数据库配置
  database:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: secret
    max-connections: 20
    pool:
      max-idle: 10
      min-idle: 5
      max-total: 50

  # 服务器配置
  server:
    hosts:
      - host1.example.com
      - host2.example.com
    ports:
      http: 8080
      https: 8443
    features:
      - name: cache
        enabled: true
      - name: metrics
        enabled: true

  # 功能开关
  feature:
    enabled: true
  cache:
    enabled: true
  redis:
    enabled: true

# Spring Boot 核心配置
spring:
  application:
    name: my-application
server:
  port: 8080
```

### 创建自定义自动配置
```java
// 1. 创建自动配置类
package com.example.autoconfigure;

import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Bean;
import org.springframework.boot.autoconfigure.condition.*;

@Configuration
@ConditionalOnClass(MyService.class)
@EnableConfigurationProperties(MyServiceProperties.class)
public class MyServiceAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public MyService myService(MyServiceProperties properties) {
        MyService service = new MyService();
        service.setUrl(properties.getUrl());
        service.setTimeout(properties.getTimeout());
        return service;
    }
}

// 2. 创建配置属性类
package com.example.autoconfigure;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "myapp.myservice")
public class MyServiceProperties {
    
    private String url = "http://localhost:8080";
    private int timeout = 5000;
    private boolean enabled = true;
    
    // getter 和 setter
    public String getUrl() { return url; }
    public void setUrl(String url) { this.url = url; }
    public int getTimeout() { return timeout; }
    public void setTimeout(int timeout) { this.timeout = timeout; }
    public boolean isEnabled() { return enabled; }
    public void setEnabled(boolean enabled) { this.enabled = enabled; }
}

// 3. 创建 spring.factories 文件（META-INF/spring.factories）
/*
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.autoconfigure.MyServiceAutoConfiguration
*/

// 4. 创建配置元数据（META-INF/spring-configuration-metadata.json）
/*
{
  "groups": [
    {
      "name": "myapp.myservice",
      "type": "com.example.autoconfigure.MyServiceProperties",
      "sourceType": "com.example.autoconfigure.MyServiceProperties"
    }
  ],
  "properties": [
    {
      "name": "myapp.myservice.url",
      "type": "java.lang.String",
      "description": "Service URL.",
      "sourceType": "com.example.autoconfigure.MyServiceProperties",
      "defaultValue": "http://localhost:8080"
    },
    {
      "name": "myapp.myservice.timeout",
      "type": "java.lang.Integer",
      "description": "Request timeout in milliseconds.",
      "sourceType": "com.example.autoconfigure.MyServiceProperties",
      "defaultValue": 5000
    },
    {
      "name": "myapp.myservice.enabled",
      "type": "java.lang.Boolean",
      "description": "Whether to enable the service.",
      "sourceType": "com.example.autoconfigure.MyServiceProperties",
      "defaultValue": true
    }
  ],
  "hints": []
}
*/

// 5. 使用自定义 starter
// pom.xml
/*
<dependency>
    <groupId>com.example</groupId>
    <artifactId>my-spring-boot-starter</artifactId>
    <version>1.0.0</version>
</dependency>
*/

// application.properties
/*
myapp.myservice.url=http://api.example.com
myapp.myservice.timeout=10000
myapp.myservice.enabled=true
*/

// 在应用中使用
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class MyController {
    
    @Autowired
    private MyService myService;
    
    @GetMapping("/test")
    public String test() {
        return myService.doSomething();
    }
}
```

### 调试自动配置
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.autoconfigure.logging.ConditionEvaluationReportLoggingListener;

// 启用自动配置报告（调试模式）
@SpringBootApplication
public class DebugApplication {

    public static void main(String[] args) {
        // 方式 1: 使用 --debug 参数
        // java -jar app.jar --debug
        
        // 方式 2: 配置属性
        // debug=true
        
        // 方式 3: 编程方式启用
        SpringApplication app = new SpringApplication(DebugApplication.class);
        app.addListeners(new ConditionEvaluationReportLoggingListener());
        app.run(args);
    }
}

// 查看自动配置报告
/*
启动应用后，控制台会输出：

============================
CONDITIONS EVALUATION REPORT
============================

Positive matches:
-----------------
   ...

Negative matches:
-----------------
   ...

Exclusions:
-----------
   ...

Unconditional classes:
----------------------
   ...
*/

// 获取自动配置报告（编程方式）
import org.springframework.boot.autoconfigure.condition.ConditionEvaluationReport;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ConfigurableApplicationContext;
import org.springframework.boot.SpringApplication;

public class AutoConfigReport {
    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(MyApp.class, args);
        
        ConditionEvaluationReport report = context.getBean(ConditionEvaluationReport.class);
        
        System.out.println("Positive matches: " + report.getConditionAndOutcomesBySource());
        System.out.println("Negative matches: " + report.getExclusions());
    }
}
```

## 4. 官方示例代码（可运行）

### 完整的自定义 Starter 示例
```java
// ==================== 项目结构 ====================
/*
my-spring-boot-starter/
├── pom.xml
└── src/
    └── main/
        ├── java/
        │   └── com/
        │       └── example/
        │           └── starter/
        │               ├── GreetingService.java
        │               ├── GreetingProperties.java
        │               └── GreetingAutoConfiguration.java
        └── resources/
            └── META-INF/
                ├── spring.factories
                └── spring-configuration-metadata.json
*/

// ==================== 1. pom.xml ====================
/*
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>greeting-spring-boot-starter</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <name>Greeting Spring Boot Starter</name>
    <description>Custom Spring Boot Starter for Greeting Service</description>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.14</version>
        <relativePath/>
    </parent>
    
    <properties>
        <java.version>11</java.version>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-autoconfigure</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>
</project>
*/

// ==================== 2. GreetingService.java ====================
package com.example.starter;

public class GreetingService {
    
    private String prefix;
    private String suffix;
    private boolean uppercase;
    
    public String greet(String name) {
        String message = prefix + " " + name + suffix;
        return uppercase ? message.toUpperCase() : message;
    }
    
    // getter 和 setter
    public String getPrefix() { return prefix; }
    public void setPrefix(String prefix) { this.prefix = prefix; }
    public String getSuffix() { return suffix; }
    public void setSuffix(String suffix) { this.suffix = suffix; }
    public boolean isUppercase() { return uppercase; }
    public void setUppercase(boolean uppercase) { this.uppercase = uppercase; }
}

// ==================== 3. GreetingProperties.java ====================
package com.example.starter;

import org.springframework.boot.context.properties.ConfigurationProperties;

@ConfigurationProperties(prefix = "greeting")
public class GreetingProperties {
    
    private String prefix = "Hello";
    private String suffix = "!";
    private boolean uppercase = false;
    private boolean enabled = true;
    
    // getter 和 setter
    public String getPrefix() { return prefix; }
    public void setPrefix(String prefix) { this.prefix = prefix; }
    public String getSuffix() { return suffix; }
    public void setSuffix(String suffix) { this.suffix = suffix; }
    public boolean isUppercase() { return uppercase; }
    public void setUppercase(boolean uppercase) { this.uppercase = uppercase; }
    public boolean isEnabled() { return enabled; }
    public void setEnabled(boolean enabled) { this.enabled = enabled; }
}

// ==================== 4. GreetingAutoConfiguration.java ====================
package com.example.starter;

import org.springframework.boot.autoconfigure.condition.ConditionalOnMissingBean;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.boot.context.properties.EnableConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
@EnableConfigurationProperties(GreetingProperties.class)
@ConditionalOnProperty(prefix = "greeting", name = "enabled", havingValue = "true", matchIfMissing = true)
public class GreetingAutoConfiguration {

    @Bean
    @ConditionalOnMissingBean
    public GreetingService greetingService(GreetingProperties properties) {
        GreetingService service = new GreetingService();
        service.setPrefix(properties.getPrefix());
        service.setSuffix(properties.getSuffix());
        service.setUppercase(properties.isUppercase());
        return service;
    }
}

// ==================== 5. spring.factories ====================
/*
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
com.example.starter.GreetingAutoConfiguration
*/

// ==================== 6. spring-configuration-metadata.json ====================
/*
{
  "groups": [
    {
      "name": "greeting",
      "type": "com.example.starter.GreetingProperties",
      "sourceType": "com.example.starter.GreetingProperties"
    }
  ],
  "properties": [
    {
      "name": "greeting.prefix",
      "type": "java.lang.String",
      "description": "Greeting prefix.",
      "sourceType": "com.example.starter.GreetingProperties",
      "defaultValue": "Hello"
    },
    {
      "name": "greeting.suffix",
      "type": "java.lang.String",
      "description": "Greeting suffix.",
      "sourceType": "com.example.starter.GreetingProperties",
      "defaultValue": "!"
    },
    {
      "name": "greeting.uppercase",
      "type": "java.lang.Boolean",
      "description": "Whether to convert greeting to uppercase.",
      "sourceType": "com.example.starter.GreetingProperties",
      "defaultValue": false
    },
    {
      "name": "greeting.enabled",
      "type": "java.lang.Boolean",
      "description": "Whether to enable greeting service.",
      "sourceType": "com.example.starter.GreetingProperties",
      "defaultValue": true
    }
  ],
  "hints": []
}
*/

// ==================== 使用自定义 Starter ====================

// pom.xml
/*
<dependency>
    <groupId>com.example</groupId>
    <artifactId>greeting-spring-boot-starter</artifactId>
    <version>1.0.0</version>
</dependency>
*/

// application.properties
/*
greeting.prefix=Hi
greeting.suffix=!!
greeting.uppercase=true
greeting.enabled=true
*/

// 在应用中使用
import com.example.starter.GreetingService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class GreetingApplication {

    @Autowired
    private GreetingService greetingService;

    public static void main(String[] args) {
        SpringApplication.run(GreetingApplication.class, args);
    }

    @GetMapping("/greet/{name}")
    public String greet(@PathVariable String name) {
        return greetingService.greet(name);
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 自动配置最佳实践
1. **理解自动配置的条件**：自动配置是条件化的，只有满足特定条件才会生效
2. **查看自动配置报告**：使用 --debug 参数查看哪些自动配置生效了
3. **合理使用 exclude**：只排除真正不需要的自动配置
4. **创建自定义配置**：当自动配置不满足需求时，提供自己的配置

### 配置属性最佳实践
1. **使用 @ConfigurationProperties**：类型安全的配置比 @Value 更好
2. **提供合理的默认值**：配置属性应该有合理的默认值
3. **使用嵌套类组织配置**：相关的配置属性应该组织在一起
4. **添加配置元数据**：为 IDE 提供自动补全支持

### 自定义 Starter 最佳实践
1. **命名规范**：官方 starter 使用 spring-boot-starter-*，自定义使用 *-spring-boot-starter
2. **单一职责**：每个 starter 应该只提供一个特定功能
3. **条件化配置**：使用条件注解让配置更灵活
4. **提供默认配置**：starter 应该开箱即用
5. **文档完善**：提供清晰的使用文档和配置示例

### 常见问题及解决
1. **自动配置不生效**：
   - 检查是否满足自动配置的条件
   - 查看自动配置报告
   - 确认类路径下有必要的依赖

2. **配置属性不绑定**：
   - 检查 @ConfigurationProperties 的 prefix 是否正确
   - 确认 getter/setter 方法存在
   - 使用 @EnableConfigurationProperties 或 @Component 注册

3. **自定义 starter 不工作**：
   - 检查 META-INF/spring.factories 是否正确
   - 确认自动配置类的条件注解
   - 检查依赖是否正确引入

4. **性能问题**：
   - 排除不需要的自动配置
   - 合理使用条件注解
   - 避免过度使用 @ConditionalOnBean

## 6. 官方文档参考链接

- Spring Boot 官方文档：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/
- 自动配置：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.auto-configuration
- 开发自定义 Starter：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.developing-auto-configuration
- @ConfigurationProperties：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config.typesafe-configuration-properties
- 条件注解：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#boot-features-condition-annotations
- 外部化配置：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#features.external-config
- Spring Boot GitHub：https://github.com/spring-projects/spring-boot
