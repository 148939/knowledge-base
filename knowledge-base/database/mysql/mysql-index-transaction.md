# MySQL 索引与事务

## 1. 核心概述（官方定义）

### 索引概述
索引（Index）是帮助 MySQL 高效获取数据的数据结构。索引是存储引擎层实现的，不同存储引擎有不同的索引类型。

### 事务概述
事务（Transaction）是访问并可能操作各种数据项的一个数据库操作序列，这些操作要么全部执行，要么全部不执行，是一个不可分割的工作单位。

### ACID 特性
事务具有四个特性：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）、持久性（Durability）。

---

## 2. 核心知识点（结构化层级）

### 2.1 索引数据结构

#### B+ 树索引
- B+ 树是 B 树的变种，叶子节点之间有双向指针
- 所有数据都在叶子节点，非叶子节点只存储索引
- 范围查询效率高，适合磁盘存储

#### Hash 索引
- 基于哈希表实现，等值查询非常快
- 不支持范围查询和排序
- Memory 引擎默认使用 Hash 索引

#### 全文索引
- 用于全文搜索，支持自然语言模式和布尔模式
- 只能在 CHAR、VARCHAR、TEXT 类型上创建

#### R-Tree 索引
- 用于空间数据类型（GEOMETRY、POINT 等）
- MyISAM 引擎支持

### 2.2 索引类型

#### 普通索引
- 最基本的索引，没有任何限制
- 可以在一个或多个字段上创建

#### 唯一索引
- 索引列的值必须唯一，但允许空值
- 可以有多个唯一索引

#### 主键索引
- 特殊的唯一索引，不允许空值
- 一个表只能有一个主键索引

#### 复合索引
- 在多个字段上创建的索引
- 使用时遵循最左前缀原则

#### 前缀索引
- 只对字段的前一部分字符建立索引
- 适合长文本字段

### 2.3 索引操作

#### 创建索引
```sql
CREATE INDEX index_name ON table_name (column1, column2, ...);
CREATE UNIQUE INDEX index_name ON table_name (column1, column2, ...);
ALTER TABLE table_name ADD INDEX index_name (column1, column2, ...);
```

#### 删除索引
```sql
DROP INDEX index_name ON table_name;
ALTER TABLE table_name DROP INDEX index_name;
```

#### 查看索引
```sql
SHOW INDEX FROM table_name;
```

### 2.4 索引使用原则

#### 索引设计原则
- 选择区分度高的列作为索引
- 频繁作为 WHERE、ORDER BY、GROUP BY 条件的列适合建索引
- 小表不需要索引
- 避免对经常更新的列建立过多索引

#### 索引失效场景
- 使用函数或表达式运算
- 类型隐式转换
- LIKE 查询以 % 开头
- OR 条件中有非索引列
- 违反最左前缀原则

#### 索引监控
- 使用 EXPLAIN 分析查询执行计划
- 查看慢查询日志
- 监控索引使用情况

### 2.5 事务特性

#### 原子性（Atomicity）
- 事务中的所有操作要么全部成功，要么全部失败
- 通过 Undo Log 实现

#### 一致性（Consistency）
- 事务执行前后，数据库从一个一致性状态转换到另一个一致性状态
- 由应用层保证

#### 隔离性（Isolation）
- 多个事务并发执行时，一个事务的执行不应影响其他事务
- 通过锁机制和 MVCC 实现

#### 持久性（Durability）
- 事务一旦提交，其结果就是永久性的
- 通过 Redo Log 实现

### 2.6 事务隔离级别

#### 读未提交（Read Uncommitted）
- 一个事务可以读取另一个未提交事务的数据
- 会出现脏读、不可重复读、幻读

#### 读已提交（Read Committed）
- 一个事务只能读取另一个已提交事务的数据
- 解决脏读，会出现不可重复读、幻读
- Oracle、SQL Server 默认级别

#### 可重复读（Repeatable Read）
- 同一事务内多次读取同一数据的结果一致
- 解决脏读、不可重复读，会出现幻读
- MySQL InnoDB 默认级别

#### 串行化（Serializable）
- 事务串行执行，最高隔离级别
- 解决所有问题，但性能最差

### 2.7 事务并发问题

#### 脏读（Dirty Read）
- 一个事务读取到另一个未提交事务的数据

#### 不可重复读（Non-repeatable Read）
- 同一事务内，多次读取同一数据结果不一致
- 主要是 UPDATE 操作导致

#### 幻读（Phantom Read）
- 同一事务内，多次查询返回的行数不一致
- 主要是 INSERT、DELETE 操作导致

### 2.8 MVCC 多版本并发控制

#### 版本链
- 每行数据有多个版本
- 通过 Undo Log 实现

#### Read View
- 判断当前事务能看到哪个版本
- 不同隔离级别 Read View 生成时机不同

#### 隐式字段
- DB_TRX_ID：最近修改事务 ID
- DB_ROLL_PTR：回滚指针
- DB_ROW_ID：隐藏主键（如果没有显式主键）

### 2.9 锁机制

#### 全局锁
- 锁整个数据库
- FLUSH TABLES WITH READ LOCK

#### 表级锁
- 锁整张表
- 读锁（共享锁）、写锁（排他锁）

#### 行级锁
- 锁单行数据
- 共享锁（S）、排他锁（X）

#### 间隙锁
- 锁一个范围，不包含记录本身
- 防止幻读

#### Next-Key Lock
- 间隙锁 + 记录锁
- InnoDB 默认锁机制

---

## 3. 官方标准语法 / API

### 3.1 索引创建语法

```sql
-- 普通索引
CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX index_name
[index_type]
ON table_name (key_part, ...)
[index_option]
[algorithm_option | lock_option] ...;

-- 主键索引
ALTER TABLE table_name ADD PRIMARY KEY (column_list);

-- 复合索引
CREATE INDEX index_name ON table_name (col1, col2, col3);

-- 前缀索引
CREATE INDEX index_name ON table_name (col1(length));

-- 全文索引
CREATE FULLTEXT INDEX index_name ON table_name (col1);
```

### 3.2 事务控制语法

```sql
-- 开始事务
START TRANSACTION;
-- 或
BEGIN;

-- 提交事务
COMMIT;

-- 回滚事务
ROLLBACK;

-- 设置保存点
SAVEPOINT savepoint_name;

-- 回滚到保存点
ROLLBACK TO SAVEPOINT savepoint_name;

-- 删除保存点
RELEASE SAVEPOINT savepoint_name;

-- 设置事务隔离级别
SET TRANSACTION ISOLATION LEVEL
{
    READ UNCOMMITTED
  | READ COMMITTED
  | REPEATABLE READ
  | SERIALIZABLE
};

-- 查看当前隔离级别
SELECT @@tx_isolation;
SELECT @@transaction_isolation;

-- 查看自动提交状态
SELECT @@autocommit;

-- 设置自动提交
SET autocommit = 0|1;
```

### 3.3 EXPLAIN 语法

```sql
EXPLAIN [FORMAT = format] SELECT ...;
EXPLAIN ANALYZE SELECT ...; -- MySQL 8.0+

-- format: TRADITIONAL|JSON|TREE
```

---

## 4. 官方示例代码（可运行）

### 4.1 索引使用示例

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
    UNIQUE INDEX idx_email (email),
    INDEX idx_age_status (age, status),
    INDEX idx_created_at (created_at)
) ENGINE=InnoDB;

-- 插入测试数据
INSERT INTO users (username, email, age, status) VALUES
('zhangsan', 'zhangsan@example.com', 25, 1),
('lisi', 'lisi@example.com', 30, 1),
('wangwu', 'wangwu@example.com', 28, 0);

-- 使用 EXPLAIN 分析
EXPLAIN SELECT * FROM users WHERE username = 'zhangsan';
EXPLAIN SELECT * FROM users WHERE age = 25 AND status = 1;

-- 复合索引最左前缀原则
-- 有效：age 或 age + status
-- 无效：仅 status

-- 添加索引
ALTER TABLE users ADD INDEX idx_status (status);

-- 删除索引
DROP INDEX idx_status ON users;

-- 查看索引
SHOW INDEX FROM users;
```

### 4.2 事务示例

```sql
-- 1. 基础事务示例
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

-- 检查是否成功
SELECT * FROM accounts WHERE id IN (1, 2);

-- 提交
COMMIT;

-- 2. 回滚示例
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 模拟错误
-- UPDATE accounts SET balance = balance + 100 WHERE id = 999;

-- 回滚
ROLLBACK;

-- 3. 保存点示例
START TRANSACTION;

INSERT INTO users (username, email) VALUES ('user1', 'user1@example.com');
SAVEPOINT sp1;

INSERT INTO users (username, email) VALUES ('user2', 'user2@example.com');
SAVEPOINT sp2;

INSERT INTO users (username, email) VALUES ('user3', 'user3@example.com');

-- 回滚到 sp2，user3 被撤销
ROLLBACK TO SAVEPOINT sp2;

COMMIT;

-- 4. 设置隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
START TRANSACTION;
-- ...
COMMIT;
```

### 4.3 锁机制示例

```sql
-- 会话1：加排他锁
START TRANSACTION;
SELECT * FROM users WHERE id = 1 FOR UPDATE;
-- 此时其他会话无法修改 id=1 的记录

-- 会话2：尝试修改（会阻塞）
UPDATE users SET age = 30 WHERE id = 1;

-- 会话1：提交释放锁
COMMIT;

-- 会话2：现在可以执行

-- 共享锁示例
START TRANSACTION;
SELECT * FROM users WHERE id = 1 LOCK IN SHARE MODE;
-- 其他会话可以读，但不能写

-- 查看锁等待
SHOW ENGINE INNODB STATUS;
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 索引最佳实践

1. **选择合适的索引列**
   - 选择区分度高的列
   - 选择频繁查询的列
   - 避免选择更新频繁的列

2. **复合索引设计**
   - 遵循最左前缀原则
   - 将区分度高的列放前面
   - 避免列数过多（一般不超过 5 个）

3. **避免索引失效**
   - 不要在索引列上使用函数
   - 避免隐式类型转换
   - LIKE 查询不要以 % 开头
   - OR 条件确保所有列都有索引

4. **索引维护**
   - 定期分析表：ANALYZE TABLE
   - 定期优化表：OPTIMIZE TABLE
   - 删除未使用的索引
   - 监控索引使用情况

### 5.2 事务最佳实践

1. **事务大小控制**
   - 事务越短越好
   - 避免大事务
   - 合理设置保存点

2. **隔离级别选择**
   - 大多数场景使用 READ COMMITTED
   - 需要一致性保证使用 REPEATABLE READ
   - 仅在必要时使用 SERIALIZABLE

3. **并发控制**
   - 合理使用锁
   - 避免死锁
   - 监控锁等待

4. **错误处理**
   - 捕获异常并回滚
   - 不要在事务中进行耗时操作
   - 确保资源正确释放

### 5.3 性能优化

1. **查询优化**
   - 使用 EXPLAIN 分析
   - 避免 SELECT *
   - 合理使用 LIMIT
   - 避免全表扫描

2. **索引优化**
   - 覆盖索引
   - 延迟关联
   - 索引条件下推
   - 索引合并

3. **配置优化**
   - innodb_buffer_pool_size
   - innodb_log_file_size
   - sync_binlog
   - innodb_flush_log_at_trx_commit

---

## 6. 官方文档参考链接

- [MySQL 官方文档 - 索引](https://dev.mysql.com/doc/refman/8.0/en/optimization-indexes.html)
- [MySQL 官方文档 - 事务](https://dev.mysql.com/doc/refman/8.0/en/sql-transactional-statements.html)
- [MySQL 官方文档 - InnoDB 锁机制](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
- [MySQL 官方文档 - MVCC](https://dev.mysql.com/doc/refman/8.0/en/innodb-multi-versioning.html)
- [MySQL 官方文档 - EXPLAIN](https://dev.mysql.com/doc/refman/8.0/en/explain.html)
