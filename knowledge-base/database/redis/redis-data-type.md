# 检索索引：本文档收录【Redis】【数据类型】知识点，内容100%来自Redis官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Redis 是一个开源的内存数据结构存储系统，可以用作数据库、缓存和消息中间件。Redis 支持多种数据类型，每种数据类型都有其特定的用途和操作命令。理解 Redis 的数据类型是高效使用 Redis 的基础。

Redis 的核心特性：
1. **基于内存**：数据存储在内存中，读写速度极快
2. **持久化支持**：支持 RDB 和 AOF 两种持久化方式
3. **多种数据结构**：支持 String、Hash、List、Set、ZSet 等
4. **单线程模型**：基于单线程的事件驱动模型
5. **高可用**：支持主从复制、哨兵和集群模式

## 2. 核心知识点（结构化层级）

### 2.1 String（字符串）
1. 基本概念和用途
2. SET、GET 命令
3. 数值操作（INCR、DECR、INCRBY、DECRBY）
4. 位操作（SETBIT、GETBIT、BITCOUNT、BITOP）
5. 批量操作（MSET、MGET）
6. 应用场景

### 2.2 Hash（哈希）
1. 基本概念和用途
2. HSET、HGET、HMSET、HMGET 命令
3. HGETALL、HKEYS、HVALS 命令
4. HINCRBY、HINCRBYFLOAT 命令
5. HEXISTS、HLEN 命令
6. 应用场景

### 2.3 List（列表）
1. 基本概念和用途
2. LPUSH、RPUSH、LPOP、RPOP 命令
3. LRANGE、LINDEX、LLEN 命令
4. LINSERT、LSET、LREM 命令
5. LPUSHX、RPUSHX、LTRIM 命令
6. BLPOP、BRPOP 阻塞命令
7. 应用场景

### 2.4 Set（集合）
1. 基本概念和用途
2. SADD、SREM、SISMEMBER 命令
3. SMEMBERS、SCARD 命令
4. SRANDMEMBER、SPOP 命令
5. SINTER、SUNION、SDIFF 命令
6. SINTERSTORE、SUNIONSTORE、SDIFFSTORE 命令
7. 应用场景

### 2.5 Sorted Set（有序集合）
1. 基本概念和用途
2. ZADD、ZREM、ZSCORE 命令
3. ZINCRBY、ZRANK、ZREVRANK 命令
4. ZRANGE、ZREVRANGE、ZRANGEBYSCORE 命令
5. ZCOUNT、ZCARD、ZREMRANGEBYRANK 命令
6. ZREMRANGEBYSCORE、ZUNIONSTORE、ZINTERSTORE 命令
7. 应用场景

### 2.6 其他数据类型
1. Bitmap（位图）
2. HyperLogLog
3. Geo（地理位置）
4. Stream（流）
5. Bitfield

## 3. 官方标准语法 / API

### String 类型操作
```bash
# 设置键值对
SET key value
SET key value EX 10  # 设置过期时间10秒
SET key value PX 10000  # 设置过期时间10000毫秒
SET key value NX  # 只有key不存在时才设置
SET key value XX  # 只有key存在时才设置

# 获取值
GET key

# 设置并返回旧值
GETSET key newvalue

# 批量设置
MSET key1 value1 key2 value2 key3 value3

# 批量获取
MGET key1 key2 key3

# 数值自增
INCR key
INCRBY key increment
DECR key
DECRBY key decrement
INCRBYFLOAT key increment

# 字符串追加
APPEND key value

# 获取字符串长度
STRLEN key

# 获取子字符串
GETRANGE key start end

# 设置子字符串
SETRANGE key offset value

# 位操作
SETBIT key offset value
GETBIT key offset
BITCOUNT key
BITCOUNT key start end
BITOP AND destkey key1 key2
BITOP OR destkey key1 key2
BITOP XOR destkey key1 key2
BITOP NOT destkey key
```

### Hash 类型操作
```bash
# 设置字段值
HSET key field value
HSET key field1 value1 field2 value2

# 获取字段值
HGET key field

# 批量设置
HMSET key field1 value1 field2 value2

# 批量获取
HMGET key field1 field2

# 获取所有字段和值
HGETALL key

# 获取所有字段
HKEYS key

# 获取所有值
HVALS key

# 检查字段是否存在
HEXISTS key field

# 删除字段
HDEL key field1 field2

# 获取字段数量
HLEN key

# 数值自增
HINCRBY key field increment
HINCRBYFLOAT key field increment
```

### List 类型操作
```bash
# 从左边插入
LPUSH key value1 value2 value3

# 从右边插入
RPUSH key value1 value2 value3

# 只有key存在时才从左边插入
LPUSHX key value

# 只有key存在时才从右边插入
RPUSHX key value

# 从左边弹出
LPOP key

# 从右边弹出
RPOP key

# 获取列表长度
LLEN key

# 获取列表指定范围的元素
LRANGE key start stop
LRANGE key 0 -1  # 获取所有元素

# 通过索引获取元素
LINDEX key index

# 通过索引设置元素
LSET key index value

# 在指定元素前/后插入新元素
LINSERT key BEFORE|AFTER pivot value

# 删除指定数量的元素
LREM key count value

# 截取列表，只保留指定范围的元素
LTRIM key start stop

# 从一个列表的右边弹出，从另一个列表的左边插入
RPOPLPUSH source destination

# 阻塞式弹出
BLPOP key1 key2 timeout
BRPOP key1 key2 timeout
BRPOPLPUSH source destination timeout
```

### Set 类型操作
```bash
# 添加元素
SADD key member1 member2 member3

# 移除元素
SREM key member1 member2

# 检查元素是否存在
SISMEMBER key member

# 获取所有元素
SMEMBERS key

# 获取元素数量
SCARD key

# 随机获取一个或多个元素
SRANDMEMBER key
SRANDMEMBER key count

# 随机弹出一个或多个元素
SPOP key
SPOP key count

# 交集
SINTER key1 key2 key3

# 并集
SUNION key1 key2 key3

# 差集（key1有但key2没有）
SDIFF key1 key2

# 交集存储到新key
SINTERSTORE destination key1 key2 key3

# 并集存储到新key
SUNIONSTORE destination key1 key2 key3

# 差集存储到新key
SDIFFSTORE destination key1 key2
```

### Sorted Set 类型操作
```bash
# 添加元素或更新分数
ZADD key score1 member1 score2 member2
ZADD key NX score member  # 只有不存在时才添加
ZADD key XX score member  # 只有存在时才更新
ZADD key CH score member  # 返回变更数量
ZADD key INCR score member  # 分数自增

# 移除元素
ZREM key member1 member2

# 获取元素分数
ZSCORE key member

# 增加元素分数
ZINCRBY key increment member

# 获取元素排名（从小到大）
ZRANK key member

# 获取元素排名（从大到小）
ZREVRANK key member

# 获取指定排名范围的元素（从小到大）
ZRANGE key start stop
ZRANGE key start stop WITHSCORES  # 同时返回分数

# 获取指定排名范围的元素（从大到小）
ZREVRANGE key start stop
ZREVRANGE key start stop WITHSCORES

# 获取指定分数范围的元素
ZRANGEBYSCORE key min max
ZRANGEBYSCORE key min max WITHSCORES
ZRANGEBYSCORE key min max LIMIT offset count

# 获取指定分数范围的元素（从大到小）
ZREVRANGEBYSCORE key max min
ZREVRANGEBYSCORE key max min WITHSCORES

# 获取分数在指定范围内的元素数量
ZCOUNT key min max

# 获取元素数量
ZCARD key

# 移除指定排名范围的元素
ZREMRANGEBYRANK key start stop

# 移除指定分数范围的元素
ZREMRANGEBYSCORE key min max

# 多个有序集合的并集
ZUNIONSTORE destination numkeys key1 key2 key3
ZUNIONSTORE destination numkeys key1 key2 WEIGHTS w1 w2 AGGREGATE SUM|MIN|MAX

# 多个有序集合的交集
ZINTERSTORE destination numkeys key1 key2 key3
ZINTERSTORE destination numkeys key1 key2 WEIGHTS w1 w2 AGGREGATE SUM|MIN|MAX
```

## 4. 官方示例代码（可运行）

### String 类型示例
```bash
# 基本键值对
SET user:1:name "张三"
GET user:1:name
# 返回: "张三"

# 设置过期时间
SET session:abc123 "user123" EX 3600
GET session:abc123
# 返回: "user123"

# 计数器
INCR page:views
# 返回: 1
INCR page:views
# 返回: 2
INCRBY page:views 10
# 返回: 12

# 批量操作
MSET user:2:name "李四" user:2:age 25 user:2:email "lisi@example.com"
MGET user:2:name user:2:age user:2:email
# 返回: 1) "李四" 2) "25" 3) "lisi@example.com"

# 位操作 - 统计在线用户
SETBIT online:users:20240101 1001 1
SETBIT online:users:20240101 1002 1
SETBIT online:users:20240101 1005 1
BITCOUNT online:users:20240101
# 返回: 3
```

### Hash 类型示例
```bash
# 用户信息存储
HSET user:1 name "张三" age 28 email "zhangsan@example.com"
# 返回: 3 (设置的字段数量)

HGET user:1 name
# 返回: "张三"

HGETALL user:1
# 返回: 1) "name" 2) "张三" 3) "age" 4) "28" 5) "email" 6) "zhangsan@example.com"

# 批量获取
HMGET user:1 name age email
# 返回: 1) "张三" 2) "28" 3) "zhangsan@example.com"

# 数值自增 - 商品库存
HSET product:1 stock 100
HINCRBY product:1 stock -5
# 返回: 95
HGET product:1 stock
# 返回: "95"

# 检查字段是否存在
HEXISTS user:1 phone
# 返回: 0 (不存在)

# 删除字段
HDEL user:1 email
# 返回: 1

# 获取字段数量
HLEN user:1
# 返回: 2
```

### List 类型示例
```bash
# 消息队列
LPUSH message:queue "消息1"
LPUSH message:queue "消息2"
LPUSH message:queue "消息3"
# 列表内容: ["消息3", "消息2", "消息1"]

# 获取所有消息
LRANGE message:queue 0 -1
# 返回: 1) "消息3" 2) "消息2" 3) "消息1"

# 获取列表长度
LLEN message:queue
# 返回: 3

# 从右边消费消息
RPOP message:queue
# 返回: "消息1"

# 最新动态 - 限制只保留10条
LPUSH news:latest "新闻1"
LPUSH news:latest "新闻2"
LPUSH news:latest "新闻3"
LTRIM news:latest 0 9  # 只保留前10条

# 栈操作
LPUSH stack "A"
LPUSH stack "B"
LPUSH stack "C"
LPOP stack
# 返回: "C"

# 阻塞队列 - 用于任务分发
# 消费者1执行:
BLPOP task:queue 0
# 阻塞等待...

# 生产者执行:
LPUSH task:queue "任务1"
# 消费者1立即收到: "任务1"
```

### Set 类型示例
```bash
# 用户标签
SADD user:1:tags "编程" "读书" "旅行"
SADD user:2:tags "编程" "音乐" "运动"

# 检查用户是否有某个标签
SISMEMBER user:1:tags "编程"
# 返回: 1

# 获取用户所有标签
SMEMBERS user:1:tags
# 返回: 1) "编程" 2) "读书" 3) "旅行"

# 获取两个用户的共同标签
SINTER user:1:tags user:2:tags
# 返回: 1) "编程"

# 获取所有标签（并集）
SUNION user:1:tags user:2:tags
# 返回: 1) "编程" 2) "读书" 3) "旅行" 4) "音乐" 5) "运动"

# 用户1有但用户2没有的标签
SDIFF user:1:tags user:2:tags
# 返回: 1) "读书" 2) "旅行"

# 抽奖系统
SADD lottery:users 1001 1002 1003 1004 1005
SRANDMEMBER lottery:users 2
# 随机抽取2个用户

# 随机抽取并移除
SPOP lottery:users
# 返回: 随机一个用户ID
```

### Sorted Set 类型示例
```bash
# 排行榜
ZADD leaderboard 100 "玩家A" 200 "玩家B" 150 "玩家C" 300 "玩家D"

# 获取前3名（从大到小）
ZREVRANGE leaderboard 0 2 WITHSCORES
# 返回: 1) "玩家D" 2) "300" 3) "玩家B" 4) "200" 5) "玩家C" 6) "150"

# 获取玩家排名
ZREVRANK leaderboard "玩家B"
# 返回: 1 (排名从0开始)

# 获取玩家分数
ZSCORE leaderboard "玩家A"
# 返回: "100"

# 增加玩家分数
ZINCRBY leaderboard 50 "玩家A"
# 返回: "150"

# 获取分数在100-200之间的玩家
ZRANGEBYSCORE leaderboard 100 200 WITHSCORES
# 返回: 1) "玩家A" 2) "150" 3) "玩家C" 4) "150" 5) "玩家B" 6) "200"

# 文章点赞
ZADD article:1001:likes 1712345678 "user1" 1712345679 "user2"

# 获取点赞用户数量
ZCARD article:1001:likes
# 返回: 2

# 获取时间范围内的点赞
ZRANGEBYSCORE article:1001:likes 1712345600 1712345700
# 返回: 1) "user1" 2) "user2"

# 延迟任务队列
ZADD delayed:tasks 1712346000 "发送邮件" 1712347000 "清理缓存"
# 查询到期任务
ZRANGEBYSCORE delayed:tasks -inf $(date +%s)
```

## 5. 官方注意事项 / 最佳实践

### String 类型最佳实践
1. **合理使用过期时间**：缓存数据务必设置过期时间
2. **避免大 Key**：单个 String 类型的值不要超过 512MB
3. **计数器使用 INCR**：原子操作，避免并发问题
4. **批量操作使用 MSET/MGET**：减少网络往返
5. **位操作适合统计**：Bitmap 适合用户签到、在线统计等场景

### Hash 类型最佳实践
1. **适合存储对象**：用户信息、配置信息等使用 Hash
2. **避免大 Hash**：单个 Hash 的字段数不要过多
3. **数值字段使用 HINCRBY**：原子增减操作
4. **考虑内存占用**：Hash 相比 String 更省内存
5. **合理使用 HGETALL**：字段多时注意性能

### List 类型最佳实践
1. **用作消息队列**：LPUSH + BRPOP 实现消息队列
2. **用作栈**：LPUSH + LPOP 实现栈
3. **限制列表长度**：使用 LTRIM 避免无限增长
4. **阻塞操作超时**：BLPOP 等阻塞命令设置合理超时
5. **列表索引性能**：避免使用过大的索引

### Set 类型最佳实践
1. **适合去重**：利用 Set 的去重特性
2. **社交关系**：好友、关注等关系使用 Set
3. **交集并集差集**：利用集合运算实现共同好友等功能
4. **随机抽样**：SRANDMEMBER 和 SPOP 适合抽奖
5. **避免大 Set**：元素过多时注意性能

### Sorted Set 类型最佳实践
1. **排行榜首选**：分数作为排序依据，实现排行榜
2. **延迟队列**：时间戳作为分数，实现延迟任务
3. **范围查询高效**：ZRANGEBYSCORE 等范围查询性能好
4. **排名计算**：ZRANK 和 ZREVRANK 快速获取排名
5. **注意分数精度**：浮点数作为分数时注意精度问题

### 通用最佳实践
1. **Key 命名规范**：使用有意义的 Key，如 `user:1:profile`
2. **Key 过期策略**：合理设置 TTL，避免内存泄漏
3. **内存优化**：选择合适的数据类型节省内存
4. **持久化配置**：根据业务需求配置 RDB/AOF
5. **监控和告警**：监控内存使用、Key 数量等指标
6. **避免单 Key 过大**：大 Key 会影响性能和迁移

## 6. 官方文档参考链接

- Redis 官方文档：https://redis.io/docs/
- Redis 数据类型：https://redis.io/docs/data-types/
- Redis 命令参考：https://redis.io/commands/
- Redis 最佳实践：https://redis.io/docs/manual/patterns/
- Redis GitHub：https://github.com/redis/redis
