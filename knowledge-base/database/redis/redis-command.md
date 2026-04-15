# 检索索引：本文档收录【Redis】【常用命令】知识点，内容100%来自Redis官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Redis 提供了丰富的命令集，用于操作各种数据类型和管理 Redis 服务器。掌握 Redis 常用命令是高效使用 Redis 的关键。Redis 命令分为多个类别，包括键操作、字符串操作、哈希操作、列表操作、集合操作、有序集合操作、发布订阅、事务、脚本等。

Redis 命令的特点：
1. **原子性**：单个命令是原子的
2. **简单易用**：命令语法简洁直观
3. **功能强大**：涵盖数据操作、管理、监控等功能
4. **高性能**：单线程执行，避免锁竞争

## 2. 核心知识点（结构化层级）

### 2.1 键（Key）操作命令
1. DEL - 删除键
2. EXISTS - 检查键是否存在
3. EXPIRE - 设置过期时间
4. TTL - 获取剩余过期时间
5. PERSIST - 移除过期时间
6. TYPE - 获取键的数据类型
7. KEYS - 查找键（生产环境慎用）
8. SCAN - 迭代键（推荐）
9. RENAME - 重命名键
10. MOVE - 移动键到其他数据库

### 2.2 服务器管理命令
1. PING - 测试连接
2. INFO - 获取服务器信息
3. CONFIG GET/SET - 配置管理
4. FLUSHDB - 清空当前数据库
5. FLUSHALL - 清空所有数据库
6. SELECT - 切换数据库
7. DBSIZE - 获取键数量
8. SAVE/BGSAVE - 持久化
9. LASTSAVE - 获取上次保存时间
10. CLIENT LIST/KILL - 客户端管理

### 2.3 发布订阅命令
1. PUBLISH - 发布消息
2. SUBSCRIBE - 订阅频道
3. UNSUBSCRIBE - 取消订阅
4. PSUBSCRIBE - 模式订阅
5. PUNSUBSCRIBE - 取消模式订阅
6. PUBSUB - 查看发布订阅状态

### 2.4 事务命令
1. MULTI - 开启事务
2. EXEC - 执行事务
3. DISCARD - 取消事务
4. WATCH - 监视键
5. UNWATCH - 取消监视

### 2.5 脚本命令
1. EVAL - 执行 Lua 脚本
2. EVALSHA - 执行脚本 SHA
3. SCRIPT LOAD - 加载脚本
4. SCRIPT EXISTS - 检查脚本是否存在
5. SCRIPT FLUSH - 清空脚本缓存
6. SCRIPT KILL - 杀死脚本

### 2.6 持久化命令
1. SAVE - 同步保存
2. BGSAVE - 后台保存
3. BGREWRITEAOF - 重写 AOF
4. LASTSAVE - 获取上次保存时间

## 3. 官方标准语法 / API

### 键操作命令
```bash
# 删除键
DEL key1 key2 key3
# 返回: 删除的键数量

# 检查键是否存在
EXISTS key
# 返回: 1 存在，0 不存在

# 设置过期时间（秒）
EXPIRE key 60
# 返回: 1 成功，0 失败

# 设置过期时间（毫秒）
PEXPIRE key 60000
# 返回: 1 成功，0 失败

# 设置过期时间（时间戳，秒）
EXPIREAT key 1712345678
# 返回: 1 成功，0 失败

# 获取剩余过期时间（秒）
TTL key
# 返回: -2 键不存在，-1 没有过期时间，>=0 剩余秒数

# 获取剩余过期时间（毫秒）
PTTL key
# 返回: -2 键不存在，-1 没有过期时间，>=0 剩余毫秒数

# 移除过期时间
PERSIST key
# 返回: 1 成功，0 失败

# 获取键的数据类型
TYPE key
# 返回: string, hash, list, set, zset 等

# 查找键（生产环境慎用，会阻塞服务器）
KEYS pattern
KEYS user:*
KEYS *
# 返回: 匹配的键列表

# 迭代键（推荐，不会阻塞服务器）
SCAN cursor
SCAN cursor MATCH pattern
SCAN cursor MATCH pattern COUNT count
SCAN 0 MATCH user:* COUNT 10
# 返回: [新的游标, 键列表]

# 重命名键
RENAME oldkey newkey
# 返回: OK

# 重命名键（仅当新键不存在时）
RENAMENX oldkey newkey
# 返回: 1 成功，0 失败

# 移动键到其他数据库
MOVE key db
# 返回: 1 成功，0 失败

# 复制键
COPY sourcekey destkey
COPY sourcekey destkey DB db
COPY sourcekey destkey REPLACE
# 返回: 1 成功，0 失败

# 键的内部编码
OBJECT ENCODING key
OBJECT IDLETIME key
OBJECT REFCOUNT key
```

### 服务器管理命令
```bash
# 测试连接
PING
# 返回: PONG

PING message
# 返回: message

# 获取服务器信息
INFO
INFO server
INFO clients
INFO memory
INFO persistence
INFO stats
INFO replication
INFO cpu
INFO commandstats
INFO cluster
INFO keyspace
# 返回: 服务器信息

# 获取配置
CONFIG GET parameter
CONFIG GET *
CONFIG GET maxmemory
# 返回: [参数名, 参数值]

# 设置配置
CONFIG SET parameter value
CONFIG SET maxmemory 1gb
CONFIG SET save "900 1 300 10 60 10000"
# 返回: OK

# 重写配置文件
CONFIG REWRITE
# 返回: OK

# 重置统计信息
CONFIG RESETSTAT
# 返回: OK

# 清空当前数据库
FLUSHDB
FLUSHDB ASYNC
# 返回: OK

# 清空所有数据库
FLUSHALL
FLUSHALL ASYNC
# 返回: OK

# 切换数据库（0-15）
SELECT 0
SELECT 1
# 返回: OK

# 获取当前数据库键数量
DBSIZE
# 返回: 键数量

# 同步保存到磁盘（会阻塞）
SAVE
# 返回: OK

# 后台保存到磁盘（异步）
BGSAVE
# 返回: Background saving started

# 后台重写 AOF 文件
BGREWRITEAOF
# 返回: Background append only file rewriting started

# 获取上次保存时间（Unix 时间戳）
LASTSAVE
# 返回: 时间戳

# 查看客户端列表
CLIENT LIST
CLIENT LIST TYPE normal
CLIENT LIST TYPE master
CLIENT LIST TYPE replica
# 返回: 客户端信息列表

# 杀死客户端
CLIENT KILL addr ip:port
CLIENT KILL ID client-id
CLIENT KILL TYPE normal
CLIENT KILL TYPE master
CLIENT KILL TYPE replica
CLIENT KILL USER username
CLIENT KILL SKIPME yes/no
# 返回: 杀死的客户端数量

# 获取当前客户端信息
CLIENT INFO
# 返回: 当前客户端信息

# 设置客户端名称
CLIENT SETNAME my-connection
# 返回: OK

# 获取客户端名称
CLIENT GETNAME
# 返回: 客户端名称

# 暂停客户端
CLIENT PAUSE timeout
CLIENT PAUSE timeout WRITE
CLIENT PAUSE timeout ALL
# 返回: OK

# 服务器时间
TIME
# 返回: [秒, 微秒]

# 慢日志
SLOWLOG GET
SLOWLOG GET 10
SLOWLOG LEN
SLOWLOG RESET
# 返回: 慢日志信息

# 查看命令
COMMAND
COMMAND COUNT
COMMAND INFO command1 command2
COMMAND DOCS
COMMAND LIST
COMMAND LIST FILTERBY module module-name
COMMAND LIST FILTERBY category category-name
COMMAND GETKEYS command arg1 arg2
COMMAND GETKEYSANDFLAGS command arg1 arg2
```

### 发布订阅命令
```bash
# 发布消息
PUBLISH channel message
PUBLISH news "Hello World"
# 返回: 订阅者数量

# 订阅频道
SUBSCRIBE channel1 channel2 channel3
SUBSCRIBE news sports
# 返回: 订阅确认

# 取消订阅
UNSUBSCRIBE channel1 channel2
UNSUBSCRIBE
# 返回: 取消订阅确认

# 模式订阅（使用通配符）
PSUBSCRIBE pattern1 pattern2
PSUBSCRIBE news.*
PSUBSCRIBE *
# 返回: 模式订阅确认

# 取消模式订阅
PUNSUBSCRIBE pattern1 pattern2
PUNSUBSCRIBE
# 返回: 取消模式订阅确认

# 查看发布订阅状态
PUBSUB CHANNELS
PUBSUB CHANNELS pattern
PUBSUB NUMSUB channel1 channel2
PUBSUB NUMPAT
# 返回: 频道列表/订阅数量/模式数量
```

### 事务命令
```bash
# 开启事务
MULTI
# 返回: OK

# 事务中的命令（入队，不执行）
SET key1 value1
INCR key2
# 返回: QUEUED

# 执行事务
EXEC
# 返回: 所有命令的执行结果列表

# 取消事务
DISCARD
# 返回: OK

# 监视键（事务前使用，如果键被修改则事务失败）
WATCH key1 key2
# 返回: OK

# 取消监视
UNWATCH
# 返回: OK

# 事务示例
WATCH mykey
MULTI
SET mykey newvalue
EXEC
# 如果 mykey 在 WATCH 后被修改，EXEC 返回 nil
```

### 脚本命令
```bash
# 执行 Lua 脚本
EVAL script numkeys key1 key2 ... arg1 arg2 ...

# 简单示例
EVAL "return 'Hello World'" 0
# 返回: "Hello World"

# 访问键和参数
EVAL "return {KEYS[1], KEYS[2], ARGV[1], ARGV[2]}" 2 key1 key2 arg1 arg2
# 返回: ["key1", "key2", "arg1", "arg2"]

# 计数器示例
EVAL "local current = redis.call('GET', KEYS[1]); if current == false then current = 0 end; current = current + ARGV[1]; redis.call('SET', KEYS[1], current); return current" 1 mycounter 5
# 返回: 5, 10, 15...

# 执行脚本 SHA（避免重复传输脚本）
EVALSHA sha1 numkeys key1 key2 ... arg1 arg2 ...

# 加载脚本到缓存
SCRIPT LOAD script
SCRIPT LOAD "return 'Hello'"
# 返回: 脚本的 SHA1 值

# 检查脚本是否存在
SCRIPT EXISTS sha1 sha2 ...
# 返回: 1/0 的列表

# 清空脚本缓存
SCRIPT FLUSH
SCRIPT FLUSH SYNC
SCRIPT FLUSH ASYNC
# 返回: OK

# 杀死正在执行的脚本
SCRIPT KILL
# 返回: OK

# 调试脚本
EVAL script numkeys ... DEBUG YES
```

### 持久化命令
```bash
# 同步保存（RDB）
SAVE
# 返回: OK

# 后台保存（RDB）
BGSAVE
# 返回: Background saving started

# 后台重写 AOF
BGREWRITEAOF
# 返回: Background append only file rewriting started

# 获取上次保存时间
LASTSAVE
# 返回: Unix 时间戳

# 检查 RDB 是否在进行中
INFO persistence
# 查看 rdb_bgsave_in_progress 字段

# 检查 AOF 是否在重写中
INFO persistence
# 查看 aof_rewrite_in_progress 字段

# 手动触发 AOF 重写（与 BGREWRITEAOF 相同）
BGREWRITEAOF
```

## 4. 官方示例代码（可运行）

### 键操作示例
```bash
# 创建测试数据
SET user:1:name "张三"
SET user:1:age "28"
SET user:2:name "李四"

# 检查键是否存在
EXISTS user:1:name
# 返回: 1

EXISTS user:1:phone
# 返回: 0

# 设置过期时间（60秒）
EXPIRE user:1:age 60
# 返回: 1

# 查看剩余时间
TTL user:1:age
# 返回: 55 (假设过了5秒)

# 查看数据类型
TYPE user:1:name
# 返回: string

# 查找键（慎用）
KEYS user:*
# 返回: 1) "user:1:name" 2) "user:1:age" 3) "user:2:name"

# 使用 SCAN 迭代键（安全）
SCAN 0 MATCH user:* COUNT 10
# 返回: 1) "0" 2) 1) "user:1:name" 2) "user:1:age" 3) "user:2:name"

# 重命名键
RENAME user:2:name user:2:username
# 返回: OK

# 查看所有键
KEYS *
# 返回: 1) "user:1:name" 2) "user:1:age" 3) "user:2:username"

# 删除键
DEL user:1:age
# 返回: 1

# 删除多个键
DEL user:1:name user:2:username
# 返回: 2
```

### 发布订阅示例
```bash
# ========== 订阅者终端 ==========
# 订阅 news 频道
SUBSCRIBE news
# 输出:
# 1) "subscribe"
# 2) "news"
# 3) (integer) 1

# 等待消息...

# ========== 发布者终端 ==========
# 发布消息到 news 频道
PUBLISH news "今天天气不错"
# 返回: 1 (有1个订阅者)

# ========== 订阅者终端收到消息 ==========
# 输出:
# 1) "message"
# 2) "news"
# 3) "今天天气不错"

# ========== 模式订阅示例 ==========
# 订阅者终端
PSUBSCRIBE news.*
# 输出:
# 1) "psubscribe"
# 2) "news.*"
# 3) (integer) 1

# 发布者终端
PUBLISH news.sports "篮球比赛"
PUBLISH news.tech "AI 新闻"

# 订阅者终端会收到两条消息
```

### 事务示例
```bash
# 正常事务示例
MULTI
SET counter 0
INCR counter
INCR counter
EXEC
# 返回: 1) OK 2) (integer) 1 3) (integer) 2

GET counter
# 返回: "2"

# 取消事务示例
MULTI
SET test "value"
DISCARD
# 返回: OK

GET test
# 返回: (nil)

# WATCH 示例 - 乐观锁
# 终端1
WATCH balance
MULTI
SET balance 100
EXEC
# 返回: 1) OK

# 终端2（在终端1执行 WATCH 后，EXEC 前）
SET balance 200
# 返回: OK

# 终端1继续执行
EXEC
# 返回: (nil) （因为 balance 被修改了，事务失败）

GET balance
# 返回: "200"
```

### Lua 脚本示例
```bash
# 简单脚本
EVAL "return 1 + 1" 0
# 返回: (integer) 2

# 访问 KEYS 和 ARGV
EVAL "return {KEYS[1], ARGV[1]}" 1 mykey myvalue
# 返回: 1) "mykey" 2) "myvalue"

# 原子性计数器（防止竞态条件）
EVAL "
  local key = KEYS[1]
  local increment = tonumber(ARGV[1])
  local current = redis.call('GET', key)
  if current == false then
    current = 0
  else
    current = tonumber(current)
  end
  current = current + increment
  redis.call('SET', key, current)
  return current
" 1 likes 5
# 第一次返回: 5
# 第二次返回: 10
# ...

# 限制请求频率（滑动窗口）
EVAL "
  local key = KEYS[1]
  local window = tonumber(ARGV[1])
  local limit = tonumber(ARGV[2])
  local now = tonumber(ARGV[3])
  
  redis.call('ZREMRANGEBYSCORE', key, 0, now - window)
  local count = redis.call('ZCARD', key)
  
  if count < limit then
    redis.call('ZADD', key, now, now)
    redis.call('EXPIRE', key, window)
    return 1
  else
    return 0
  end
" 1 rate_limit:user:123 60 10 $(date +%s)
# 返回: 1 允许，0 限制
```

## 5. 官方注意事项 / 最佳实践

### 键操作最佳实践
1. **不要使用 KEYS**：生产环境禁止使用 KEYS，使用 SCAN 代替
2. **合理设置过期时间**：缓存数据务必设置 TTL
3. **键名规范**：使用有意义的键名，如 `user:1:profile`
4. **避免大 Key**：单个键的值不要过大
5. **定期清理**：定期清理过期和无用的键

### 服务器管理最佳实践
1. **INFO 命令**：定期使用 INFO 监控服务器状态
2. **慢日志**：开启慢日志，监控慢查询
3. **配置持久化**：根据业务需求配置 RDB/AOF
4. **限制内存**：设置 maxmemory，防止内存溢出
5. **监控客户端**：使用 CLIENT LIST 监控连接

### 发布订阅最佳实践
1. **消息可靠性**：发布订阅不保证消息可靠，需要可靠性考虑使用 Stream
2. **模式订阅**：合理使用模式订阅，但避免过于复杂的模式
3. **消息大小**：单个消息不要过大
4. **监控订阅者**：使用 PUBSUB 命令监控订阅状态
5. **考虑使用 Stream**：需要消息持久化、ACK 等功能时使用 Stream

### 事务最佳实践
1. **WATCH 使用**：使用 WATCH 实现乐观锁
2. **事务原子性**：Redis 事务是原子的，但不支持回滚
3. **错误处理**：事务中命令出错会继续执行后续命令
4. **避免长事务**：事务执行期间会阻塞其他命令
5. **考虑使用 Lua**：复杂逻辑优先考虑 Lua 脚本

### Lua 脚本最佳实践
1. **原子性**：Lua 脚本是原子执行的
2. **避免长时间运行**：脚本执行期间会阻塞其他命令
3. **使用 EVALSHA**：多次执行的脚本使用 EVALSHA 减少网络传输
4. **传入键和参数**：通过 KEYS 和 ARGV 传入，不要硬编码
5. **脚本调试**：使用 DEBUG 模式调试脚本
6. **超时处理**：长时间运行的脚本可能被 SCRIPT KILL 终止

### 持久化最佳实践
1. **RDB + AOF 组合**：重要数据建议同时使用 RDB 和 AOF
2. **AOF 重写**：定期重写 AOF 文件
3. **禁用同步**：根据业务需求选择合适的 fsync 策略
4. **备份策略**：定期备份 RDB 文件
5. **监控持久化**：监控 BGSAVE 和 BGREWRITEAOF 的执行情况

### 通用最佳实践
1. **命令批量执行**：使用管道或事务减少网络往返
2. **连接池**：使用连接池管理连接
3. **超时设置**：设置合理的连接超时和命令超时
4. **监控告警**：监控内存、CPU、连接数、慢查询等
5. **安全配置**：配置密码、绑定地址、禁用危险命令
6. **版本升级**：定期升级 Redis 版本，获取新特性和安全修复

## 6. 官方文档参考链接

- Redis 命令参考：https://redis.io/commands/
- Redis 官方文档：https://redis.io/docs/
- Redis 键操作：https://redis.io/commands/?group=generic
- Redis 服务器：https://redis.io/commands/?group=server
- Redis 发布订阅：https://redis.io/commands/?group=pubsub
- Redis 事务：https://redis.io/docs/manual/transactions/
- Redis Lua 脚本：https://redis.io/docs/manual/programmability/eval-intro/
- Redis 持久化：https://redis.io/docs/manual/persistence/
- Redis GitHub：https://github.com/redis/redis
