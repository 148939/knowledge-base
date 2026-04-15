# MySQL 性能优化

## 1. 核心概述（官方定义）

MySQL 性能优化是通过调整配置参数、优化查询、设计合理的索引、规范表结构等手段，提高 MySQL 数据库的查询性能、吞吐量和响应时间的过程。

性能优化的目标是：
- 减少查询响应时间
- 提高系统吞吐量
- 降低资源消耗（CPU、内存、磁盘 IO）
- 提升系统稳定性

---

## 2. 核心知识点（结构化层级）

### 2.1 查询优化

#### 慢查询分析
- 启用慢查询日志
- 分析慢查询日志
- 使用 mysqldumpslow 工具
- 使用 pt-query-digest 工具

#### EXPLAIN 执行计划
- id：查询序列号
- select_type：查询类型
- table：访问的表
- type：访问类型
- possible_keys：可能使用的索引
- key：实际使用的索引
- key_len：索引长度
- ref：索引比较的列
- rows：预计扫描行数
- Extra：额外信息

#### 查询优化原则
- 避免 SELECT *
- 合理使用 LIMIT
- 避免在 WHERE 子句中使用函数
- 避免隐式类型转换
- 使用 UNION ALL 代替 UNION
- 合理使用子查询和 JOIN

### 2.2 索引优化

#### 索引设计原则
- 选择区分度高的列
- 复合索引遵循最左前缀原则
- 避免过多索引
- 考虑查询频率
- 考虑更新频率

#### 索引使用技巧
- 覆盖索引
- 延迟关联
- 索引条件下推（ICP）
- 索引合并
- 前缀索引

#### 索引监控与维护
- 查看索引使用情况
- 分析表（ANALYZE TABLE）
- 优化表（OPTIMIZE TABLE）
- 检查索引碎片
- 删除未使用的索引

### 2.3 表结构优化

#### 数据类型选择
- 选择最小的数据类型
- 避免 NULL 值
- 合理使用 ENUM
- 合理使用 VARCHAR 和 CHAR
- 使用整数类型存储 IP

#### 表设计原则
- 范式与反范式
- 垂直拆分
- 水平拆分
- 合理使用分区表

#### 字符集与排序规则
- 选择合适的字符集（utf8mb4）
- 选择合适的排序规则
- 统一字符集避免转换

### 2.4 配置优化

#### 内存配置
- innodb_buffer_pool_size
- innodb_log_file_size
- innodb_log_buffer_size
- key_buffer_size（MyISAM）
- query_cache_size（MySQL 8.0 已废弃）

#### 连接配置
- max_connections
- back_log
- wait_timeout
- interactive_timeout
- thread_cache_size

#### IO 配置
- innodb_flush_log_at_trx_commit
- sync_binlog
- innodb_flush_method
- innodb_read_io_threads
- innodb_write_io_threads

#### 日志配置
- slow_query_log
- long_query_time
- log_queries_not_using_indexes
- binlog_format
- expire_logs_days

### 2.5 存储引擎优化

#### InnoDB 优化
- 主键设计
- 事务优化
- 锁优化
- MVCC 优化
- 缓冲池优化

#### MyISAM 优化
- 索引优化
- 表锁定
- 压缩表
- 延迟更新索引键

#### 存储引擎选择
- InnoDB：默认选择，支持事务、行锁、外键
- MyISAM：只读场景，全文索引
- Memory：临时数据，高速缓存

### 2.6 锁优化

#### 锁类型
- 表级锁
- 行级锁
- 共享锁（S）
- 排他锁（X）
- 意向锁
- 间隙锁
- Next-Key Lock

#### 锁优化策略
- 尽量使用索引
- 减少锁持有时间
- 降低隔离级别
- 合理设计事务
- 避免死锁

#### 锁监控
- SHOW ENGINE INNODB STATUS
- INFORMATION_SCHEMA.INNODB_LOCKS
- INFORMATION_SCHEMA.INNODB_LOCK_WAITS
- 死锁日志

### 2.7 复制优化

#### 主从复制原理
- 二进制日志（binlog）
- 中继日志（relay log）
- 复制线程（IO 线程、SQL 线程）

#### 复制类型
- 异步复制
- 半同步复制
- 增强半同步复制
- 组复制

#### 复制优化
- 合理配置 binlog
- 并行复制
- 半同步复制
- 读写分离
- 复制监控

### 2.8 分库分表

#### 垂直拆分
- 按业务拆分
- 按字段拆分
- 优缺点分析

#### 水平拆分
- 范围分片
- Hash 分片
- 一致性 Hash
- 分库分表中间件

#### 分库分表中间件
- ShardingSphere
- MyCat
- Vitess
- TDDL

---

## 3. 官方标准语法 / API

### 3.1 慢查询配置

```sql
-- 启用慢查询日志
SET GLOBAL slow_query_log = 'ON';

-- 设置慢查询阈值（秒）
SET GLOBAL long_query_time = 2;

-- 记录未使用索引的查询
SET GLOBAL log_queries_not_using_indexes = 'ON';

-- 设置慢查询日志文件路径
SET GLOBAL slow_query_log_file = '/var/log/mysql/slow-query.log';

-- 查看慢查询配置
SHOW VARIABLES LIKE 'slow_query%';
SHOW VARIABLES LIKE 'long_query_time';
```

### 3.2 EXPLAIN 语法

```sql
-- 基本 EXPLAIN
EXPLAIN SELECT * FROM users WHERE id = 1;

-- JSON 格式
EXPLAIN FORMAT=JSON SELECT * FROM users WHERE id = 1;

-- TREE 格式（MySQL 8.0+）
EXPLAIN FORMAT=TREE SELECT * FROM users WHERE id = 1;

-- ANALYZE（MySQL 8.0+）
EXPLAIN ANALYZE SELECT * FROM users WHERE id = 1;

-- SHOW WARNINGS 查看优化器提示
EXPLAIN SELECT * FROM users WHERE id = 1;
SHOW WARNINGS;
```

### 3.3 性能配置语法

```sql
-- 查看配置
SHOW VARIABLES;
SHOW VARIABLES LIKE 'innodb%';

-- 动态修改配置（部分参数）
SET GLOBAL innodb_buffer_pool_size = 1073741824;
SET GLOBAL max_connections = 200;

-- 查看状态
SHOW STATUS;
SHOW STATUS LIKE 'Innodb%';
SHOW STATUS LIKE 'Threads%';

-- 查看进程列表
SHOW PROCESSLIST;
SHOW FULL PROCESSLIST;

-- 查看表信息
SHOW TABLE STATUS LIKE 'users';

-- 分析表
ANALYZE TABLE users;

-- 优化表
OPTIMIZE TABLE users;

-- 检查表
CHECK TABLE users;

-- 修复表
REPAIR TABLE users;
```

### 3.4 锁监控语法

```sql
-- 查看 InnoDB 引擎状态
SHOW ENGINE INNODB STATUS;

-- 查看锁信息（MySQL 5.7）
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCKS;
SELECT * FROM INFORMATION_SCHEMA.INNODB_LOCK_WAITS;

-- 查看锁信息（MySQL 8.0+）
SELECT * FROM performance_schema.data_locks;
SELECT * FROM performance_schema.data_lock_waits;

-- 查看事务
SELECT * FROM INFORMATION_SCHEMA.INNODB_TRX;

-- 查看锁等待
SHOW PROCESSLIST;
```

---

## 4. 官方示例代码（可运行）

### 4.1 EXPLAIN 使用示例

```sql
-- 创建测试表
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    email VARCHAR(100) NOT NULL,
    age INT,
    status TINYINT DEFAULT 1,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    INDEX idx_username (username),
    INDEX idx_age_status (age, status),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB;

-- 插入测试数据
INSERT INTO users (username, email, age, status) VALUES
('zhangsan', 'zhangsan@example.com', 25, 1),
('lisi', 'lisi@example.com', 30, 1),
('wangwu', 'wangwu@example.com', 28, 0);

-- 1. 主键查询
EXPLAIN SELECT * FROM users WHERE id = 1;
-- type: const, key: PRIMARY

-- 2. 索引查询
EXPLAIN SELECT * FROM users WHERE username = 'zhangsan';
-- type: ref, key: idx_username

-- 3. 复合索引查询
EXPLAIN SELECT * FROM users WHERE age = 25 AND status = 1;
-- type: ref, key: idx_age_status

-- 4. 范围查询
EXPLAIN SELECT * FROM users WHERE age > 20 AND age < 30;
-- type: range, key: idx_age_status

-- 5. 全表扫描
EXPLAIN SELECT * FROM users WHERE status = 1;
-- type: ALL, key: NULL

-- 6. 索引失效示例
EXPLAIN SELECT * FROM users WHERE YEAR(created_at) = 2024;
-- type: ALL, 使用函数导致索引失效

-- 7. 查看 JSON 格式执行计划
EXPLAIN FORMAT=JSON SELECT * FROM users WHERE id = 1\G
```

### 4.2 慢查询分析示例

```sql
-- 1. 配置慢查询
SET GLOBAL slow_query_log = 'ON';
SET GLOBAL long_query_time = 1;
SET GLOBAL log_queries_not_using_indexes = 'ON';

-- 2. 模拟慢查询
SELECT SLEEP(2);
SELECT * FROM users WHERE status = 1;

-- 3. 使用 mysqldumpslow 分析（命令行）
-- mysqldumpslow -s t -t 10 /var/log/mysql/slow-query.log

-- 4. 查看慢查询状态
SHOW VARIABLES LIKE 'slow_query%';
SHOW STATUS LIKE 'Slow_queries';

-- 5. 性能基准测试
-- 使用 sysbench
-- sysbench oltp_read_write --mysql-host=localhost --mysql-user=root --mysql-password=xxx --mysql-db=test --table-size=10000 prepare
-- sysbench oltp_read_write --mysql-host=localhost --mysql-user=root --mysql-password=xxx --mysql-db=test --table-size=10000 run
```

### 4.3 配置优化示例

```sql
-- my.cnf 配置示例（针对 InnoDB）
-- [mysqld]
-- # 基础配置
-- port = 3306
-- datadir = /var/lib/mysql
-- socket = /var/run/mysqld/mysqld.sock
-- 
-- # 字符集
-- character-set-server = utf8mb4
-- collation-server = utf8mb4_unicode_ci
-- 
-- # 连接配置
-- max_connections = 200
-- back_log = 100
-- wait_timeout = 600
-- 
-- # 内存配置（假设 4GB 内存）
-- innodb_buffer_pool_size = 2G  # 50-70% 可用内存
-- innodb_log_file_size = 512M
-- innodb_log_buffer_size = 64M
-- 
-- # IO 配置
-- innodb_flush_log_at_trx_commit = 2
-- sync_binlog = 0
-- innodb_flush_method = O_DIRECT
-- 
-- # 慢查询
-- slow_query_log = 1
-- slow_query_log_file = /var/log/mysql/slow-query.log
-- long_query_time = 2
-- log_queries_not_using_indexes = 1

-- 查看当前配置
SHOW VARIABLES;

-- 查看 InnoDB 缓冲池状态
SHOW STATUS LIKE 'Innodb_buffer_pool%';

-- 计算缓冲池命中率
-- Hit Ratio = 1 - (Innodb_buffer_pool_reads / Innodb_buffer_pool_read_requests)
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 查询优化最佳实践

1. **避免 SELECT ***
   - 只查询需要的列
   - 减少网络传输
   - 增加覆盖索引的可能性

2. **合理使用 LIMIT**
   - 分页查询使用 LIMIT
   - 大偏移量使用延迟关联
   - 使用 WHERE 代替 LIMIT

3. **避免在 WHERE 子句中使用函数**
   - 函数会导致索引失效
   - 使用计算列或存储生成列
   - 调整查询逻辑

4. **避免隐式类型转换**
   - 确保查询条件类型与列类型一致
   - 使用显式类型转换
   - 统一字符集

5. **使用 UNION ALL 代替 UNION**
   - UNION 会去重排序，性能差
   - 确定不需要去重时使用 UNION ALL

### 5.2 索引优化最佳实践

1. **索引设计原则**
   - 选择区分度高的列
   - 复合索引遵循最左前缀
   - 避免索引过多
   - 考虑查询和更新频率

2. **索引使用技巧**
   - 使用覆盖索引减少回表
   - 大偏移量使用延迟关联
   - 前缀索引减少索引大小
   - 索引条件下推优化

3. **索引监控与维护**
   - 定期分析表：ANALYZE TABLE
   - 定期优化表：OPTIMIZE TABLE
   - 删除未使用的索引
   - 监控索引使用情况

### 5.3 配置优化最佳实践

1. **内存配置**
   - innodb_buffer_pool_size：50-70% 可用内存
   - innodb_log_file_size：256M-2G
   - 避免使用查询缓存（MySQL 8.0 已废弃）

2. **连接配置**
   - max_connections：根据并发数设置
   - wait_timeout：不要设置过长
   - thread_cache_size：减少线程创建开销

3. **IO 配置**
   - innodb_flush_log_at_trx_commit：2（性能优先）或 1（安全优先）
   - sync_binlog：0（性能优先）或 1（安全优先）
   - innodb_flush_method：O_DIRECT

4. **日志配置**
   - 启用慢查询日志
   - 合理设置 long_query_time
   - 启用 binlog 用于复制

### 5.4 锁优化最佳实践

1. **减少锁竞争**
   - 尽量使用索引
   - 避免全表扫描
   - 合理设计事务大小

2. **减少锁持有时间**
   - 事务越短越好
   - 不要在事务中进行耗时操作
   - 合理设置保存点

3. **避免死锁**
   - 统一加锁顺序
   - 减少事务大小
   - 合理设置超时
   - 监控死锁日志

### 5.5 分库分表最佳实践

1. **何时分库分表**
   - 单表数据量超过 1000 万
   - 单表数据量超过 10GB
   - 索引无法全部加载到内存
   - 查询性能下降明显

2. **分片策略选择**
   - 范围分片：适合按时间查询
   - Hash 分片：数据分布均匀
   - 一致性 Hash：支持动态扩缩容

3. **分库分表中间件**
   - ShardingSphere：功能强大，生态丰富
   - MyCat：成熟稳定，国内使用广泛
   - Vitess：YouTube 开源，适合大规模

---

## 6. 官方文档参考链接

- [MySQL 官方文档 - 优化](https://dev.mysql.com/doc/refman/8.0/en/optimization.html)
- [MySQL 官方文档 - EXPLAIN](https://dev.mysql.com/doc/refman/8.0/en/explain.html)
- [MySQL 官方文档 - InnoDB 配置](https://dev.mysql.com/doc/refman/8.0/en/innodb-configuration.html)
- [MySQL 官方文档 - 慢查询日志](https://dev.mysql.com/doc/refman/8.0/en/slow-query-log.html)
- [MySQL 官方文档 - 锁机制](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
- [MySQL 官方文档 - 复制](https://dev.mysql.com/doc/refman/8.0/en/replication.html)
