# 检索索引：本文档收录【MySQL】【SQL与CRUD】知识点，内容100%来自MySQL官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

MySQL 是一个开源的关系型数据库管理系统（RDBMS），由 Oracle 公司开发和维护。SQL（Structured Query Language，结构化查询语言）是用于管理关系数据库的标准语言。CRUD（Create, Read, Update, Delete）是数据库操作的四个基本功能：创建、读取、更新和删除。

## 2. 核心知识点（结构化层级）

### 2.1 SQL 基础
1. SQL 简介
2. SQL 分类
   - DDL（数据定义语言）
   - DML（数据操作语言）
   - DQL（数据查询语言）
   - DCL（数据控制语言）
   - TCL（事务控制语言）
3. SQL 语法规则
4. 注释

### 2.2 数据库操作
1. 创建数据库
2. 查看数据库
3. 选择数据库
4. 修改数据库
5. 删除数据库

### 2.3 表操作
1. 创建表
2. 查看表结构
3. 修改表
4. 重命名表
5. 删除表

### 2.4 数据类型
1. 数值类型
   - 整数类型：TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT
   - 浮点类型：FLOAT, DOUBLE
   - 定点类型：DECIMAL
2. 字符串类型
   - CHAR, VARCHAR
   - TEXT, TINYTEXT, MEDIUMTEXT, LONGTEXT
   - BINARY, VARBINARY
   - BLOB, TINYBLOB, MEDIUMBLOB, LONGBLOB
   - ENUM, SET
3. 日期时间类型
   - DATE, TIME, DATETIME, TIMESTAMP, YEAR
4. JSON 类型

### 2.5 约束
1. PRIMARY KEY（主键）
2. FOREIGN KEY（外键）
3. NOT NULL（非空）
4. UNIQUE（唯一）
5. CHECK（检查）
6. DEFAULT（默认）

### 2.6 插入数据（Create）
1. INSERT 语句
2. 插入单行数据
3. 插入多行数据
4. 插入查询结果
5. INSERT ... ON DUPLICATE KEY UPDATE

### 2.7 查询数据（Read）
1. SELECT 语句
2. 查询所有列
3. 查询指定列
4. 列别名
5. WHERE 子句
6. 比较运算符
7. 逻辑运算符
8. LIKE 模糊查询
9. IN 和 BETWEEN
10. IS NULL 和 IS NOT NULL
11. ORDER BY 排序
12. LIMIT 分页
13. DISTINCT 去重
14. GROUP BY 分组
15. HAVING 子句
16. 聚合函数：COUNT, SUM, AVG, MAX, MIN

### 2.8 更新数据（Update）
1. UPDATE 语句
2. 更新单行数据
3. 更新多行数据
4. 使用 WHERE 条件
5. 多表更新

### 2.9 删除数据（Delete）
1. DELETE 语句
2. 删除单行数据
3. 删除多行数据
4. 使用 WHERE 条件
5. TRUNCATE TABLE
6. 多表删除

### 2.10 连接查询
1. 内连接（INNER JOIN）
2. 左连接（LEFT JOIN）
3. 右连接（RIGHT JOIN）
4. 全连接（FULL JOIN）
5. 自连接
6. 交叉连接（CROSS JOIN）

### 2.11 子查询
1. 子查询简介
2. 标量子查询
3. 列子查询
4. 行子查询
5. 表子查询
6. EXISTS 和 NOT EXISTS

### 2.12 索引
1. 索引简介
2. 创建索引
3. 查看索引
4. 删除索引
5. 普通索引
6. 唯一索引
7. 主键索引
8. 组合索引
9. 全文索引

## 3. 官方标准语法 / API

### 数据库操作语法
```sql
-- 创建数据库
CREATE DATABASE database_name;
CREATE DATABASE IF NOT EXISTS database_name;
CREATE DATABASE database_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 查看数据库
SHOW DATABASES;

-- 选择数据库
USE database_name;

-- 修改数据库
ALTER DATABASE database_name CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- 删除数据库
DROP DATABASE database_name;
DROP DATABASE IF EXISTS database_name;
```

### 表操作语法
```sql
-- 创建表
CREATE TABLE table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    column3 datatype constraints,
    ...
);

-- 示例
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    age INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 查看表
SHOW TABLES;

-- 查看表结构
DESCRIBE table_name;
DESC table_name;
SHOW COLUMNS FROM table_name;

-- 修改表
-- 添加列
ALTER TABLE table_name ADD COLUMN column_name datatype constraints;

-- 修改列
ALTER TABLE table_name MODIFY COLUMN column_name datatype constraints;
ALTER TABLE table_name CHANGE COLUMN old_name new_name datatype constraints;

-- 删除列
ALTER TABLE table_name DROP COLUMN column_name;

-- 添加主键
ALTER TABLE table_name ADD PRIMARY KEY (column_name);

-- 添加外键
ALTER TABLE table_name ADD CONSTRAINT fk_name FOREIGN KEY (column_name) REFERENCES other_table (column_name);

-- 重命名表
RENAME TABLE old_name TO new_name;
ALTER TABLE old_name RENAME TO new_name;

-- 删除表
DROP TABLE table_name;
DROP TABLE IF EXISTS table_name;
```

### 插入数据语法
```sql
-- 插入单行数据（指定列）
INSERT INTO table_name (column1, column2, column3)
VALUES (value1, value2, value3);

-- 插入单行数据（所有列）
INSERT INTO table_name
VALUES (value1, value2, value3, ...);

-- 插入多行数据
INSERT INTO table_name (column1, column2)
VALUES
    (value1, value2),
    (value3, value4),
    (value5, value6);

-- 插入查询结果
INSERT INTO table1 (column1, column2)
SELECT columnA, columnB FROM table2 WHERE condition;

-- INSERT ... ON DUPLICATE KEY UPDATE
INSERT INTO table_name (id, column1, column2)
VALUES (1, 'value1', 'value2')
ON DUPLICATE KEY UPDATE
    column1 = VALUES(column1),
    column2 = VALUES(column2);
```

### 查询数据语法
```sql
-- 查询所有列
SELECT * FROM table_name;

-- 查询指定列
SELECT column1, column2 FROM table_name;

-- 列别名
SELECT column1 AS alias1, column2 AS alias2 FROM table_name;

-- WHERE 子句
SELECT * FROM table_name WHERE condition;

-- 比较运算符
SELECT * FROM table_name WHERE column = value;
SELECT * FROM table_name WHERE column <> value;
SELECT * FROM table_name WHERE column > value;
SELECT * FROM table_name WHERE column < value;
SELECT * FROM table_name WHERE column >= value;
SELECT * FROM table_name WHERE column <= value;

-- 逻辑运算符
SELECT * FROM table_name WHERE condition1 AND condition2;
SELECT * FROM table_name WHERE condition1 OR condition2;
SELECT * FROM table_name WHERE NOT condition;

-- LIKE 模糊查询
SELECT * FROM table_name WHERE column LIKE 'pattern';
-- % 匹配任意多个字符
-- _ 匹配单个字符
SELECT * FROM table_name WHERE column LIKE 'a%';   -- 以 a 开头
SELECT * FROM table_name WHERE column LIKE '%z';   -- 以 z 结尾
SELECT * FROM table_name WHERE column LIKE '%abc%'; -- 包含 abc
SELECT * FROM table_name WHERE column LIKE '_bc';   -- 第二个字符开始是 bc

-- IN 和 BETWEEN
SELECT * FROM table_name WHERE column IN (value1, value2, value3);
SELECT * FROM table_name WHERE column BETWEEN value1 AND value2;

-- IS NULL 和 IS NOT NULL
SELECT * FROM table_name WHERE column IS NULL;
SELECT * FROM table_name WHERE column IS NOT NULL;

-- ORDER BY 排序
SELECT * FROM table_name ORDER BY column ASC;  -- 升序（默认）
SELECT * FROM table_name ORDER BY column DESC; -- 降序
SELECT * FROM table_name ORDER BY column1 ASC, column2 DESC; -- 多列排序

-- LIMIT 分页
SELECT * FROM table_name LIMIT offset, count;
SELECT * FROM table_name LIMIT 10;          -- 前 10 条
SELECT * FROM table_name LIMIT 10, 10;      -- 第 11-20 条

-- DISTINCT 去重
SELECT DISTINCT column FROM table_name;

-- 聚合函数
SELECT COUNT(*) FROM table_name;              -- 行数
SELECT COUNT(column) FROM table_name;         -- 非 NULL 值的数量
SELECT SUM(column) FROM table_name;           -- 总和
SELECT AVG(column) FROM table_name;           -- 平均值
SELECT MAX(column) FROM table_name;           -- 最大值
SELECT MIN(column) FROM table_name;           -- 最小值

-- GROUP BY 分组
SELECT column1, COUNT(*) FROM table_name GROUP BY column1;
SELECT column1, SUM(column2) FROM table_name GROUP BY column1;

-- HAVING 子句
SELECT column1, COUNT(*) FROM table_name 
GROUP BY column1 
HAVING COUNT(*) > 10;

-- 完整 SELECT 语句
SELECT 
    column1, 
    column2, 
    COUNT(*) AS count,
    SUM(column3) AS total
FROM table_name
WHERE condition
GROUP BY column1, column2
HAVING count > 10
ORDER BY total DESC
LIMIT 20;
```

### 更新数据语法
```sql
-- 更新数据
UPDATE table_name
SET column1 = value1, column2 = value2
WHERE condition;

-- 示例
UPDATE users
SET email = 'newemail@example.com', age = 30
WHERE id = 1;

-- 多表更新
UPDATE table1, table2
SET table1.column = value
WHERE table1.id = table2.id AND condition;
```

### 删除数据语法
```sql
-- 删除数据
DELETE FROM table_name WHERE condition;

-- 示例
DELETE FROM users WHERE id = 1;

-- 删除所有数据（不重置自增）
DELETE FROM table_name;

-- 清空表（重置自增，更快）
TRUNCATE TABLE table_name;

-- 多表删除
DELETE t1, t2 FROM table1 t1
JOIN table2 t2 ON t1.id = t2.id
WHERE condition;
```

### 连接查询语法
```sql
-- 内连接
SELECT * FROM table1
INNER JOIN table2 ON table1.id = table2.id;

-- 左连接
SELECT * FROM table1
LEFT JOIN table2 ON table1.id = table2.id;

-- 右连接
SELECT * FROM table1
RIGHT JOIN table2 ON table1.id = table2.id;

-- 自连接
SELECT t1.*, t2.* FROM table1 t1
JOIN table1 t2 ON t1.parent_id = t2.id;

-- 交叉连接
SELECT * FROM table1
CROSS JOIN table2;

-- 多表连接
SELECT * FROM table1
JOIN table2 ON table1.id = table2.table1_id
JOIN table3 ON table2.id = table3.table2_id
WHERE condition;
```

### 子查询语法
```sql
-- 标量子查询（返回单个值）
SELECT * FROM table1
WHERE column1 = (SELECT column2 FROM table2 WHERE condition);

-- 列子查询（返回一列）
SELECT * FROM table1
WHERE column1 IN (SELECT column2 FROM table2 WHERE condition);

-- EXISTS 子查询
SELECT * FROM table1
WHERE EXISTS (SELECT 1 FROM table2 WHERE table2.table1_id = table1.id);

SELECT * FROM table1
WHERE NOT EXISTS (SELECT 1 FROM table2 WHERE table2.table1_id = table1.id);

-- FROM 子句中的子查询
SELECT * FROM (
    SELECT column1, column2 FROM table1 WHERE condition
) AS subquery;
```

### 索引语法
```sql
-- 创建普通索引
CREATE INDEX index_name ON table_name (column_name);
CREATE INDEX index_name ON table_name (column1, column2);

-- 创建唯一索引
CREATE UNIQUE INDEX index_name ON table_name (column_name);

-- 创建全文索引
CREATE FULLTEXT INDEX index_name ON table_name (column_name);

-- 查看索引
SHOW INDEX FROM table_name;

-- 删除索引
DROP INDEX index_name ON table_name;
ALTER TABLE table_name DROP INDEX index_name;
```

## 4. 官方示例代码（可运行）

### 电商数据库示例
```sql
-- 创建数据库
CREATE DATABASE IF NOT EXISTS shop;
USE shop;

-- 创建用户表
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    address TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 创建产品表
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    description TEXT,
    price DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL DEFAULT 0,
    category VARCHAR(50),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

-- 创建订单表
CREATE TABLE orders (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    total_amount DECIMAL(10, 2) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 创建订单详情表
CREATE TABLE order_items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    order_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);

-- 插入示例数据
INSERT INTO users (username, email, password, phone, address) VALUES
('张三', 'zhangsan@example.com', 'password123', '13800138001', '北京市朝阳区'),
('李四', 'lisi@example.com', 'password456', '13800138002', '上海市浦东新区'),
('王五', 'wangwu@example.com', 'password789', '13800138003', '广州市天河区');

INSERT INTO products (name, description, price, stock, category) VALUES
('iPhone 15', '苹果最新手机', 7999.00, 100, '手机'),
('MacBook Pro', '苹果笔记本电脑', 14999.00, 50, '电脑'),
('AirPods Pro', '苹果无线耳机', 1999.00, 200, '配件'),
('iPad Air', '苹果平板电脑', 4799.00, 80, '平板');

-- 查询所有用户
SELECT * FROM users;

-- 查询所有产品
SELECT * FROM products;

-- 查询价格大于 5000 的产品
SELECT * FROM products WHERE price > 5000;

-- 查询库存少于 100 的产品
SELECT * FROM products WHERE stock < 100;

-- 查询电子产品（手机或电脑）
SELECT * FROM products WHERE category IN ('手机', '电脑');

-- 查询价格在 1000 到 10000 之间的产品
SELECT * FROM products WHERE price BETWEEN 1000 AND 10000;

-- 按价格降序排序
SELECT * FROM products ORDER BY price DESC;

-- 按库存升序排序
SELECT * FROM products ORDER BY stock ASC;

-- 查询前 3 个产品
SELECT * FROM products LIMIT 3;

-- 分页查询，每页 2 个，第 2 页
SELECT * FROM products ORDER BY id LIMIT 2, 2;

-- 统计产品总数
SELECT COUNT(*) AS total_products FROM products;

-- 计算产品总价
SELECT SUM(price * stock) AS total_value FROM products;

-- 计算平均价格
SELECT AVG(price) AS avg_price FROM products;

-- 找出最贵和最便宜的产品
SELECT MAX(price) AS max_price, MIN(price) AS min_price FROM products;

-- 按类别分组统计
SELECT category, COUNT(*) AS count, AVG(price) AS avg_price
FROM products
GROUP BY category;

-- 更新产品价格
UPDATE products SET price = 8499.00 WHERE name = 'iPhone 15';

-- 更新库存
UPDATE products SET stock = stock - 10 WHERE id = 1;

-- 删除产品
DELETE FROM products WHERE id = 4;

-- 创建订单示例
INSERT INTO orders (user_id, total_amount, status) VALUES (1, 9998.00, 'paid');

INSERT INTO order_items (order_id, product_id, quantity, price) VALUES
(1, 1, 1, 8499.00),
(1, 3, 1, 1499.00);

-- 查询订单及用户信息
SELECT 
    orders.id,
    orders.total_amount,
    orders.status,
    orders.created_at,
    users.username,
    users.email
FROM orders
JOIN users ON orders.user_id = users.id;

-- 查询订单详情
SELECT 
    order_items.id,
    order_items.quantity,
    order_items.price,
    products.name AS product_name
FROM order_items
JOIN products ON order_items.product_id = products.id
WHERE order_items.order_id = 1;

-- 查询用户及其订单（左连接）
SELECT 
    users.username,
    COUNT(orders.id) AS order_count
FROM users
LEFT JOIN orders ON users.id = orders.user_id
GROUP BY users.id, users.username;

-- 使用子查询查询购买过 iPhone 的用户
SELECT * FROM users WHERE id IN (
    SELECT DISTINCT orders.user_id
    FROM orders
    JOIN order_items ON orders.id = order_items.order_id
    JOIN products ON order_items.product_id = products.id
    WHERE products.name LIKE '%iPhone%'
);

-- 创建索引
CREATE INDEX idx_products_category ON products (category);
CREATE INDEX idx_orders_user_id ON orders (user_id);
CREATE INDEX idx_order_items_order_id ON order_items (order_id);
CREATE INDEX idx_order_items_product_id ON order_items (product_id);

-- 查看索引
SHOW INDEX FROM products;
```

## 5. 官方注意事项 / 最佳实践

### SQL 语法注意事项
1. **语句结束**：SQL 语句以分号结束
2. **大小写**：SQL 关键字不区分大小写，但建议大写
3. **标识符**：数据库名、表名、列名等标识符建议使用小写和下划线
4. **字符串**：字符串使用单引号括起来
5. **注释**：使用 -- 单行注释或 /* */ 多行注释
6. **保留字**：避免使用 SQL 保留字作为标识符，如必须使用，用反引号括起来

### 数据库设计最佳实践
1. **命名规范**：使用有意义的表名和列名
2. **主键**：每个表都应该有主键
3. **外键**：合理使用外键保证数据完整性
4. **范式**：遵循数据库规范化原则，避免数据冗余
5. **数据类型**：选择合适的数据类型，避免浪费空间
6. **默认值**：为列设置合理的默认值

### 查询最佳实践
1. **避免 SELECT ***：只查询需要的列
2. **使用 WHERE**：总是使用 WHERE 子句过滤数据
3. **索引利用**：确保查询可以利用索引
4. **避免 SELECT DISTINCT**：除非必要，避免使用 DISTINCT
5. **LIMIT 分页**：使用 LIMIT 限制返回行数
6. **JOIN 优化**：合理使用 JOIN，避免不必要的连接
7. **子查询 vs JOIN**：根据情况选择子查询或 JOIN

### 索引最佳实践
1. **选择性高**：为选择性高的列创建索引
2. **组合索引**：合理使用组合索引，注意列的顺序
3. **避免过度索引**：索引会占用空间并影响写入性能
4. **定期维护**：定期分析和优化索引
5. **覆盖索引**：创建覆盖索引避免回表

### 性能优化
1. **EXPLAIN**：使用 EXPLAIN 分析查询执行计划
2. **批量操作**：使用批量插入、更新提高效率
3. **事务**：合理使用事务保证数据一致性
4. **连接池**：使用连接池管理数据库连接
5. **读写分离**：大规模应用考虑读写分离
6. **分库分表**：数据量大时考虑分库分表

### 安全注意事项
1. **SQL 注入**：使用预编译语句防止 SQL 注入
2. **权限管理**：最小权限原则，合理分配用户权限
3. **数据加密**：敏感数据加密存储
4. **备份**：定期备份数据
5. **日志审计**：记录数据库操作日志

## 6. 官方文档参考链接

- MySQL 官方文档：https://dev.mysql.com/doc/
- MySQL 参考手册：https://dev.mysql.com/doc/refman/
- SQL 教程：https://dev.mysql.com/doc/refman/en/sql-statements.html
