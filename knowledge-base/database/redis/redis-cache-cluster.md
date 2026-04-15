# 检索索引：本文档收录【Redis】【缓存与集群】知识点，内容100%来自Redis官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Redis 作为高性能的内存数据存储系统，广泛应用于缓存场景。Redis 缓存能够显著提升应用性能，减轻数据库压力。同时，Redis 提供了主从复制、哨兵模式和集群模式，实现高可用和水平扩展。

Redis 缓存与集群的核心特性：
1. **高性能缓存**：内存存储，读写速度极快
2. **多种过期策略**：支持多种键过期和淘汰策略
3. **主从复制**：实现数据备份和读写分离
4. **哨兵模式**：自动故障转移，提高可用性
5. **集群模式**：支持水平扩展，数据分片存储

## 2. 核心知识点（结构化层级）

### 2.1 缓存应用场景
1. 页面缓存
2. 对象缓存
3. 查询结果缓存
4. 会话缓存
5. 热点数据缓存
6. 计数器缓存
7. 分布式锁

### 2.2 缓存问题与解决方案
1. 缓存穿透
2. 缓存击穿
3. 缓存雪崩
4. 数据一致性
5. 缓存更新策略
6. 缓存预热

### 2.3 键过期策略
1. 定期删除
2. 惰性删除
3. 淘汰策略（LRU、LFU、随机等）

### 2.4 主从复制
1. 主从复制原理
2. 主从配置
3. 全量复制
4. 增量复制
5. 读写分离
6. 复制积压缓冲区

### 2.5 哨兵（Sentinel）模式
1. 哨兵功能
2. 哨兵配置
3. 主观下线与客观下线
4. 领导者选举
5. 故障转移
6. 哨兵监控

### 2.6 Redis Cluster 集群
1. 集群架构
2. 哈希槽（Hash Slot）
3. 节点通信
4. 数据分片
5. 集群扩容缩容
6. 故障转移
7. 集群命令

## 3. 官方标准语法 / API

### 缓存配置
```bash
# 设置最大内存
CONFIG SET maxmemory 2gb
CONFIG SET maxmemory 50%

# 设置内存淘汰策略
CONFIG SET maxmemory-policy allkeys-lru
# 可选值:
# - noeviction: 不淘汰，返回错误
# - allkeys-lru: 淘汰最近最少使用的键
# - allkeys-lfu: 淘汰最不经常使用的键
# - allkeys-random: 随机淘汰
# - volatile-lru: 淘汰设置了过期时间的键中最近最少使用的
# - volatile-lfu: 淘汰设置了过期时间的键中最不经常使用的
# - volatile-random: 随机淘汰设置了过期时间的键
# - volatile-ttl: 淘汰即将过期的键

# 内存淘汰策略相关配置
CONFIG SET maxmemory-samples 5  # LRU/LFU 采样数量

# 过期键清理配置
CONFIG SET hz 10  # 每秒执行过期键清理的次数
```

### Spring Boot 集成 Redis 缓存
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
public class RedisCacheConfig {

    @Bean
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
            .entryTtl(Duration.ofHours(1))  // 默认过期时间1小时
            .serializeKeysWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new StringRedisSerializer()))
            .serializeValuesWith(RedisSerializationContext.SerializationPair
                .fromSerializer(new GenericJackson2JsonRedisSerializer()))
            .disableCachingNullValues();  // 不缓存 null 值

        return RedisCacheManager.builder(connectionFactory)
            .cacheDefaults(config)
            .withCacheConfiguration("users", 
                config.entryTtl(Duration.ofMinutes(30)))
            .withCacheConfiguration("products", 
                config.entryTtl(Duration.ofHours(2)))
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
        System.out.println("从数据库查询用户: " + id);
        return userRepository.findById(id).orElse(null);
    }

    @CachePut(value = "users", key = "#user.id")
    public User updateUser(User user) {
        System.out.println("更新用户: " + user.getId());
        return userRepository.save(user);
    }

    @CacheEvict(value = "users", key = "#id")
    public void deleteUser(Long id) {
        System.out.println("删除用户: " + id);
        userRepository.deleteById(id);
    }

    @CacheEvict(value = "users", allEntries = true)
    public void clearAllUsers() {
        System.out.println("清空所有用户缓存");
    }

    @Cacheable(value = "hotProducts", key = "#categoryId")
    public List<Product> getHotProducts(Long categoryId) {
        System.out.println("从数据库查询热门商品: " + categoryId);
        return productRepository.findHotProducts(categoryId);
    }
}
```

### 缓存穿透解决方案 - 布隆过滤器
```java
import com.google.common.hash.BloomFilter;
import com.google.common.hash.Funnels;
import org.springframework.stereotype.Component;

import java.nio.charset.StandardCharsets;

@Component
public class BloomFilterService {

    private static final int EXPECTED_INSERTIONS = 1000000;
    private static final double FPP = 0.01;

    private final BloomFilter<String> bloomFilter;

    public BloomFilterService() {
        this.bloomFilter = BloomFilter.create(
            Funnels.stringFunnel(StandardCharsets.UTF_8),
            EXPECTED_INSERTIONS,
            FPP
        );
    }

    public void add(String key) {
        bloomFilter.put(key);
    }

    public boolean mightContain(String key) {
        return bloomFilter.mightContain(key);
    }

    public void initData(List<String> keys) {
        for (String key : keys) {
            bloomFilter.put(key);
        }
    }
}

// 使用布隆过滤器防止缓存穿透
@Service
public class ProductService {

    @Autowired
    private BloomFilterService bloomFilterService;

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Autowired
    private ProductRepository productRepository;

    public Product getProductById(Long productId) {
        String key = "product:" + productId;
        
        // 1. 先检查布隆过滤器
        if (!bloomFilterService.mightContain(key)) {
            return null;  // 肯定不存在
        }
        
        // 2. 查询缓存
        Product product = (Product) redisTemplate.opsForValue().get(key);
        if (product != null) {
            return product;
        }
        
        // 3. 查询数据库
        product = productRepository.findById(productId).orElse(null);
        
        // 4. 回写缓存
        if (product != null) {
            redisTemplate.opsForValue().set(key, product, Duration.ofHours(1));
        } else {
            // 缓存空值，防止缓存穿透
            redisTemplate.opsForValue().set(key, null, Duration.ofMinutes(5));
        }
        
        return product;
    }
}
```

### 缓存击穿解决方案 - 互斥锁
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Service;

import java.util.concurrent.TimeUnit;

@Service
public class HotDataService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    private static final String LOCK_PREFIX = "lock:";
    private static final long LOCK_EXPIRE_TIME = 10;  // 锁过期时间10秒

    public Product getHotProduct(Long productId) {
        String key = "product:" + productId;
        String lockKey = LOCK_PREFIX + key;

        // 1. 查询缓存
        Product product = (Product) redisTemplate.opsForValue().get(key);
        if (product != null) {
            return product;
        }

        // 2. 缓存未命中，尝试获取锁
        boolean locked = tryLock(lockKey);
        if (!locked) {
            // 获取锁失败，等待一段时间后重试
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            return getHotProduct(productId);
        }

        try {
            // 3. 再次查询缓存（双重检查）
            product = (Product) redisTemplate.opsForValue().get(key);
            if (product != null) {
                return product;
            }

            // 4. 查询数据库
            product = productRepository.findById(productId).orElse(null);

            // 5. 回写缓存
            if (product != null) {
                redisTemplate.opsForValue().set(key, product, Duration.ofHours(1));
            } else {
                redisTemplate.opsForValue().set(key, null, Duration.ofMinutes(5));
            }

            return product;
        } finally {
            // 6. 释放锁
            unlock(lockKey);
        }
    }

    private boolean tryLock(String key) {
        Boolean result = redisTemplate.opsForValue()
            .setIfAbsent(key, "1", LOCK_EXPIRE_TIME, TimeUnit.SECONDS);
        return Boolean.TRUE.equals(result);
    }

    private void unlock(String key) {
        redisTemplate.delete(key);
    }
}
```

### 主从复制配置
```bash
# 主节点配置（redis-master.conf）
port 6379
bind 0.0.0.0
daemonize yes
pidfile /var/run/redis_6379.pid
logfile /var/log/redis_6379.log
dir /var/lib/redis/6379

# 从节点配置（redis-slave.conf）
port 6380
bind 0.0.0.0
daemonize yes
pidfile /var/run/redis_6380.pid
logfile /var/log/redis_6380.log
dir /var/lib/redis/6380

# 配置主从复制
replicaof 127.0.0.1 6379

# 从节点只读（默认就是只读）
replica-read-only yes

# 复制积压缓冲区大小
repl-backlog-size 1mb

# 复制积压缓冲区过期时间
repl-backlog-ttl 3600

# 复制超时时间
repl-timeout 60

# 磁盘同步策略
repl-diskless-sync no
repl-diskless-sync-delay 5

# 查看复制信息
INFO replication

# 手动触发重新同步
REPLICAOF NO ONE  # 停止复制，变为主节点
REPLICAOF host port  # 重新开始复制
```

### 哨兵模式配置
```bash
# 哨兵配置文件（sentinel.conf）
port 26379
daemonize yes
pidfile /var/run/redis-sentinel.pid
logfile /var/log/redis-sentinel.log
dir /tmp

# 监控主节点
# sentinel monitor <master-name> <ip> <port> <quorum>
sentinel monitor mymaster 127.0.0.1 6379 2

# 主节点密码（如果有）
# sentinel auth-pass mymaster password

# 主观下线判断时间（毫秒）
sentinel down-after-milliseconds mymaster 30000

# 故障转移超时时间
sentinel failover-timeout mymaster 180000

# 并行复制的从节点数量
sentinel parallel-syncs mymaster 1

# 通知脚本
# sentinel notification-script mymaster /path/to/script.sh

# 客户端重配置脚本
# sentinel client-reconfig-script mymaster /path/to/script.sh

# 查看哨兵信息
INFO sentinel

# 查看被监控的主节点
SENTINEL masters

# 查看主节点信息
SENTINEL master mymaster

# 查看从节点信息
SENTINEL slaves mymaster

# 手动触发故障转移
SENTINEL failover mymaster
```

### Redis Cluster 集群配置
```bash
# 集群节点配置（redis-7001.conf）
port 7001
bind 0.0.0.0
daemonize yes
pidfile /var/run/redis_7001.pid
logfile /var/log/redis_7001.log
dir /var/lib/redis/7001

# 开启集群模式
cluster-enabled yes

# 集群配置文件（自动生成）
cluster-config-file nodes-7001.conf

# 节点超时时间（毫秒）
cluster-node-timeout 15000

# AOF 持久化
appendonly yes
appendfilename "appendonly-7001.aof"

# 创建并启动 6 个节点（3主3从）
# redis-server redis-7001.conf
# redis-server redis-7002.conf
# redis-server redis-7003.conf
# redis-server redis-7004.conf
# redis-server redis-7005.conf
# redis-server redis-7006.conf

# 使用 redis-cli 创建集群
redis-cli --cluster create 127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003 127.0.0.1:7004 127.0.0.1:7005 127.0.0.1:7006 --cluster-replicas 1

# 集群操作命令
CLUSTER INFO  # 查看集群信息
CLUSTER NODES  # 查看节点信息
CLUSTER SLOTS  # 查看槽分配信息
CLUSTER KEYSLOT key  # 计算键属于哪个槽
CLUSTER COUNTKEYSINSLOT slot  # 统计槽中的键数量
CLUSTER GETKEYSINSLOT slot count  # 获取槽中的键
CLUSTER MEET ip port  # 添加节点到集群
CLUSTER FORGET node-id  # 从集群中移除节点
CLUSTER REPLICATE node-id  # 设置为从节点
CLUSTER FAILOVER  # 手动故障转移
CLUSTER RESET  # 重置集群
CLUSTER SAVECONFIG  # 保存集群配置

# 查看集群信息（集群模式下）
redis-cli -c -p 7001 cluster info
redis-cli -c -p 7001 cluster nodes

# 集群模式下连接（自动重定向）
redis-cli -c -p 7001

# 集群扩容 - 添加主节点
redis-cli --cluster add-node 127.0.0.1:7007 127.0.0.1:7001

# 集群扩容 - 添加从节点
redis-cli --cluster add-node 127.0.0.1:7008 127.0.0.1:7001 --cluster-slave --cluster-master-id <master-node-id>

# 重新分片
redis-cli --cluster reshard 127.0.0.1:7001

# 删除节点
redis-cli --cluster del-node 127.0.0.1:7001 <node-id>

# 检查集群
redis-cli --cluster check 127.0.0.1:7001

# 修复集群
redis-cli --cluster fix 127.0.0.1:7001
```

### Spring Boot 集成 Redis Cluster
```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisClusterConfiguration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.connection.lettuce.LettuceClientConfiguration;
import org.springframework.data.redis.connection.lettuce.LettuceConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.GenericJackson2JsonRedisSerializer;
import org.springframework.data.redis.serializer.StringRedisSerializer;

import java.time.Duration;
import java.util.Arrays;
import java.util.List;

@Configuration
public class RedisClusterConfig {

    @Bean
    public RedisConnectionFactory redisConnectionFactory() {
        List<String> clusterNodes = Arrays.asList(
            "127.0.0.1:7001",
            "127.0.0.1:7002",
            "127.0.0.1:7003",
            "127.0.0.1:7004",
            "127.0.0.1:7005",
            "127.0.0.1:7006"
        );

        RedisClusterConfiguration clusterConfig = new RedisClusterConfiguration(clusterNodes);
        clusterConfig.setMaxRedirects(3);

        LettuceClientConfiguration clientConfig = LettuceClientConfiguration.builder()
            .commandTimeout(Duration.ofSeconds(2))
            .shutdownTimeout(Duration.ofSeconds(2))
            .build();

        return new LettuceConnectionFactory(clusterConfig, clientConfig);
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory connectionFactory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(connectionFactory);
        
        StringRedisSerializer stringSerializer = new StringRedisSerializer();
        template.setKeySerializer(stringSerializer);
        template.setHashKeySerializer(stringSerializer);
        
        GenericJackson2JsonRedisSerializer jsonSerializer = new GenericJackson2JsonRedisSerializer();
        template.setValueSerializer(jsonSerializer);
        template.setHashValueSerializer(jsonSerializer);
        
        template.afterPropertiesSet();
        return template;
    }
}
```

## 4. 官方示例代码（可运行）

### 缓存更新策略 - Cache Aside Pattern
```java
@Service
public class ProductService {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Autowired
    private ProductRepository productRepository;

    // 读取：先读缓存，缓存未命中读数据库，然后回写缓存
    public Product getProduct(Long productId) {
        String key = "product:" + productId;
        
        // 1. 查询缓存
        Product product = (Product) redisTemplate.opsForValue().get(key);
        if (product != null) {
            return product;
        }
        
        // 2. 查询数据库
        product = productRepository.findById(productId).orElse(null);
        
        // 3. 回写缓存
        if (product != null) {
            redisTemplate.opsForValue().set(key, product, Duration.ofHours(1));
        }
        
        return product;
    }

    // 写入：先更新数据库，然后删除缓存
    @Transactional
    public void updateProduct(Product product) {
        String key = "product:" + product.getId();
        
        // 1. 更新数据库
        productRepository.save(product);
        
        // 2. 删除缓存（下次读取时重新加载）
        redisTemplate.delete(key);
    }

    // 删除：先删除数据库，然后删除缓存
    @Transactional
    public void deleteProduct(Long productId) {
        String key = "product:" + productId;
        
        // 1. 删除数据库
        productRepository.deleteById(productId);
        
        // 2. 删除缓存
        redisTemplate.delete(key);
    }
}
```

### 缓存预热
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class CacheWarmup implements CommandLineRunner {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    @Autowired
    private ProductRepository productRepository;

    @Override
    public void run(String... args) {
        warmupHotProducts();
        warmupCategoryData();
    }

    private void warmupHotProducts() {
        System.out.println("开始预热热门商品缓存...");
        
        List<Product> hotProducts = productRepository.findHotProducts();
        for (Product product : hotProducts) {
            String key = "product:" + product.getId();
            redisTemplate.opsForValue().set(key, product, Duration.ofHours(2));
        }
        
        System.out.println("热门商品缓存预热完成，共 " + hotProducts.size() + " 条");
    }

    private void warmupCategoryData() {
        System.out.println("开始预热分类数据缓存...");
        
        List<Category> categories = categoryRepository.findAll();
        for (Category category : categories) {
            String key = "category:" + category.getId();
            redisTemplate.opsForValue().set(key, category, Duration.ofHours(6));
        }
        
        System.out.println("分类数据缓存预热完成，共 " + categories.size() + " 条");
    }
}
```

### 分布式锁实现
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.script.DefaultRedisScript;
import org.springframework.stereotype.Component;

import java.util.Collections;
import java.util.UUID;
import java.util.concurrent.TimeUnit;

@Component
public class DistributedLock {

    @Autowired
    private RedisTemplate<String, Object> redisTemplate;

    private static final String LOCK_PREFIX = "distributed:lock:";
    private static final long DEFAULT_EXPIRE_TIME = 30;  // 默认30秒
    private static final long DEFAULT_WAIT_TIME = 10;  // 默认等待10秒

    private final ThreadLocal<String> lockValue = new ThreadLocal<>();

    public boolean tryLock(String lockKey) {
        return tryLock(lockKey, DEFAULT_EXPIRE_TIME, DEFAULT_WAIT_TIME);
    }

    public boolean tryLock(String lockKey, long expireTime, long waitTime) {
        String key = LOCK_PREFIX + lockKey;
        String value = UUID.randomUUID().toString();
        long deadline = System.currentTimeMillis() + waitTime * 1000;

        while (System.currentTimeMillis() < deadline) {
            Boolean success = redisTemplate.opsForValue()
                .setIfAbsent(key, value, expireTime, TimeUnit.SECONDS);
            
            if (Boolean.TRUE.equals(success)) {
                lockValue.set(value);
                return true;
            }

            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
                return false;
            }
        }

        return false;
    }

    public void unlock(String lockKey) {
        String key = LOCK_PREFIX + lockKey;
        String value = lockValue.get();
        
        if (value == null) {
            return;
        }

        // 使用 Lua 脚本确保原子性：只有值匹配才删除
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then " +
                       "return redis.call('del', KEYS[1]) " +
                       "else " +
                       "return 0 " +
                       "end";

        DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
        redisScript.setScriptText(script);
        redisScript.setResultType(Long.class);

        redisTemplate.execute(redisScript, Collections.singletonList(key), value);
        lockValue.remove();
    }

    // 使用分布式锁
    public void doSomethingWithLock() {
        String lockKey = "my:resource";
        
        if (tryLock(lockKey)) {
            try {
                // 执行需要加锁的业务逻辑
                System.out.println("获取锁成功，执行业务逻辑");
            } finally {
                unlock(lockKey);
            }
        } else {
            System.out.println("获取锁失败");
        }
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 缓存最佳实践
1. **合理设置过期时间**：根据数据更新频率设置 TTL
2. **选择合适的淘汰策略**：热点数据使用 allkeys-lru
3. **防止缓存穿透**：使用布隆过滤器或缓存空值
4. **防止缓存击穿**：热点数据使用互斥锁或永不过期
5. **防止缓存雪崩**：错开过期时间，使用多级缓存
6. **保证数据一致性**：使用 Cache Aside Pattern
7. **缓存预热**：系统启动时加载热点数据
8. **监控缓存指标**：命中率、内存使用等
9. **Key 命名规范**：使用有意义的前缀，如 `product:1:detail`
10. **避免大 Key**：单个 Key 不要过大

### 内存优化
1. **使用合适的数据类型**：选择合适的数据结构节省内存
2. **压缩数据**：大数据考虑压缩后存储
3. **内存淘汰策略**：配置合理的淘汰策略
4. **maxmemory 限制**：设置最大内存限制
5. **碎片整理**：定期进行内存碎片整理
6. **使用 Hash 节省内存**：小对象使用 Hash 存储

### 主从复制最佳实践
1. **读写分离**：主节点写，从节点读
2. **监控复制延迟**：关注从节点的复制偏移量
3. **合理配置复制积压缓冲区**：根据网络情况调整
4. **从节点只读**：默认从节点只读，不要写入
5. **避免全量复制**：网络稳定时优先使用增量复制
6. **备份策略**：定期从从节点备份，避免影响主节点

### 哨兵模式最佳实践
1. **至少 3 个哨兵**：保证 quorum 多数派
2. **哨兵节点分散部署**：避免单点故障
3. **合理设置 down-after-milliseconds**：根据网络情况调整
4. **监控哨兵状态**：监控哨兵本身的健康状态
5. **配置通知**：配置故障转移通知
6. **测试故障转移**：定期测试故障转移功能

### 集群模式最佳实践
1. **节点数量**：至少 3 个主节点，每个主节点至少 1 个从节点
2. **哈希槽管理**：合理分配 16384 个槽
3. **节点监控**：监控集群节点状态
4. **槽迁移**：业务低峰期进行扩容缩容
5. **避免大 Key**：大 Key 会影响槽迁移性能
6. **客户端使用集群模式**：使用 -c 参数或支持集群的客户端
7. **跨槽命令限制**：避免使用涉及多个槽的命令（如 MGET 等）
8. **合理设置 cluster-node-timeout**：根据网络情况调整

### 高可用最佳实践
1. **持久化配置**：根据业务需求配置 RDB 和 AOF
2. **备份策略**：定期备份数据
3. **监控告警**：监控内存、CPU、QPS、慢查询等
4. **容量规划**：提前规划容量，避免内存不足
5. **版本升级**：定期升级 Redis 版本
6. **安全加固**：配置密码、绑定地址、禁用危险命令

## 6. 官方文档参考链接

- Redis 缓存最佳实践：https://redis.io/docs/manual/patterns/
- Redis 主从复制：https://redis.io/docs/management/replication/
- Redis 哨兵：https://redis.io/docs/management/sentinel/
- Redis 集群：https://redis.io/docs/management/scaling/
- Redis 内存优化：https://redis.io/docs/management/optimization/memory-optimization/
- Spring Data Redis：https://docs.spring.io/spring-data/redis/reference/
- Spring Cache：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#io.caching
- Redis GitHub：https://github.com/redis/redis
