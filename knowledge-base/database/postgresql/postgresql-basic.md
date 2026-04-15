# PostgreSQL 基础入门

## 1. 核心概述（官方定义）

PostgreSQL 是一个功能强大的开源对象关系数据库系统（ORDBMS），它使用并扩展了 SQL 语言，并结合了许多能够安全存储和扩展最复杂数据工作负载的功能。PostgreSQL 以其可靠性、健壮性和性能而闻名。

### 核心概念
- **Database**：数据库
- **Schema**：模式，数据库对象的命名空间
- **Table**：表
- **Column**：列
- **Row**：行
- **Index**：索引
- **View**：视图
- **Function**：函数
- **Trigger**：触发器
- **Sequence**：序列

---

## 2. 核心知识点（结构化层级）

### 2.1 数据类型

#### 数值类型
- SMALLINT：小整数（-32768 到 +32767）
- INTEGER：整数（-2147483648 到 +2147483647）
- BIGINT：大整数
- NUMERIC/DECIMAL：精确数值
- REAL：单精度浮点数
- DOUBLE PRECISION：双精度浮点数
- SERIAL：自增整数

#### 字符类型
- VARCHAR(n)：可变长度字符
- CHAR(n)：固定长度字符
- TEXT：无限长度文本

#### 日期/时间类型
- DATE：日期（年、月、日）
- TIME：时间（时、分、秒）
- TIMESTAMP：日期和时间
- TIMESTAMPTZ：带时区的时间戳
- INTERVAL：时间间隔

#### 布尔类型
- BOOLEAN：布尔值（TRUE/FALSE/NULL）

#### 二进制类型
- BYTEA：二进制数据

#### 数组类型
- INTEGER[]：整数数组
- TEXT[]：文本数组

#### JSON 类型
- JSON：文本格式的 JSON
- JSONB：二进制格式的 JSON（支持索引）

#### 其他类型
- UUID：通用唯一标识符
- XML：XML 数据
- 枚举类型
- 复合类型

### 2.2 基本操作

#### 数据库操作
- 创建数据库
- 删除数据库
- 连接数据库
- 查看数据库列表

#### 表操作
- 创建表
- 删除表
- 修改表结构
- 查看表结构

#### 数据操作
- 插入数据
- 查询数据
- 更新数据
- 删除数据

### 2.3 查询操作

#### 基本查询
- SELECT：选择列
- FROM：指定表
- WHERE：条件过滤
- ORDER BY：排序
- LIMIT/OFFSET：分页

#### 连接查询
- INNER JOIN：内连接
- LEFT JOIN：左连接
- RIGHT JOIN：右连接
- FULL OUTER JOIN：全外连接
- CROSS JOIN：交叉连接

#### 聚合查询
- COUNT()：计数
- SUM()：求和
- AVG()：平均值
- MAX()：最大值
- MIN()：最小值
- GROUP BY：分组
- HAVING：分组过滤

#### 子查询
- 标量子查询
- 行子查询
- 表子查询
- EXISTS 子查询

### 2.4 索引

#### 索引类型
- B-tree：默认索引类型
- Hash：哈希索引
- GiST：通用搜索树
- GIN：倒排索引
- BRIN：块范围索引
- SP-GiST：空间分区 GiST

#### 索引操作
- 创建索引
- 删除索引
- 查看索引
- 分析查询性能

### 2.5 事务

#### 事务特性
- 原子性（Atomicity）
- 一致性（Consistency）
- 隔离性（Isolation）
- 持久性（Durability）

#### 事务控制
- BEGIN/START TRANSACTION：开始事务
- COMMIT：提交事务
- ROLLBACK：回滚事务
- SAVEPOINT：保存点

#### 隔离级别
- Read Uncommitted：读未提交
- Read Committed：读已提交（默认）
- Repeatable Read：可重复读
- Serializable：可串行化

### 2.6 视图

#### 视图操作
- 创建视图
- 删除视图
- 更新视图（可更新视图）
- 物化视图

### 2.7 函数与存储过程

#### 函数
- 创建函数
- 调用函数
- 删除函数
- PL/pgSQL 语言

#### 存储过程
- 创建存储过程
- 调用存储过程
- 删除存储过程

### 2.8 触发器

#### 触发器操作
- 创建触发器
- 删除触发器
- 触发器函数
- 触发时机（BEFORE/AFTER/INSTEAD OF）
- 触发事件（INSERT/UPDATE/DELETE）

### 2.9 序列

#### 序列操作
- 创建序列
- 使用序列
- 修改序列
- 删除序列
- SERIAL 类型

### 2.10 用户与权限

#### 用户管理
- 创建用户
- 修改用户
- 删除用户
- 查看用户

#### 权限管理
- 授予权限（GRANT）
- 撤销权限（REVOKE）
- 权限类型（SELECT/INSERT/UPDATE/DELETE等）

---

## 3. 官方标准语法 / API

### 3.1 数据库操作

```sql
-- 创建数据库
CREATE DATABASE mydb;

-- 创建数据库并指定所有者
CREATE DATABASE mydb OWNER myuser;

-- 删除数据库
DROP DATABASE mydb;

-- 连接数据库（在 psql 中）
\c mydb

-- 查看所有数据库
\l
```

### 3.2 表操作

```sql
-- 创建表
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password VARCHAR(255) NOT NULL,
    age INTEGER CHECK (age >= 0),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 创建表并添加注释
COMMENT ON TABLE users IS '用户表';
COMMENT ON COLUMN users.username IS '用户名';
COMMENT ON COLUMN users.email IS '邮箱';

-- 修改表名
ALTER TABLE users RENAME TO customers;

-- 添加列
ALTER TABLE users ADD COLUMN phone VARCHAR(20);

-- 修改列类型
ALTER TABLE users ALTER COLUMN phone TYPE VARCHAR(30);

-- 删除列
ALTER TABLE users DROP COLUMN phone;

-- 添加约束
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);

-- 删除约束
ALTER TABLE users DROP CONSTRAINT unique_email;

-- 删除表
DROP TABLE users;

-- 删除表（如果存在）
DROP TABLE IF EXISTS users;

-- 查看表结构
\d users
```

### 3.3 数据操作

```sql
-- 插入数据
INSERT INTO users (username, email, password, age)
VALUES ('alice', 'alice@example.com', 'hashed_password', 25);

-- 插入多行数据
INSERT INTO users (username, email, password, age)
VALUES 
    ('bob', 'bob@example.com', 'hashed_password', 30),
    ('charlie', 'charlie@example.com', 'hashed_password', 35);

-- 插入数据并返回生成的 ID
INSERT INTO users (username, email, password, age)
VALUES ('david', 'david@example.com', 'hashed_password', 28)
RETURNING id;

-- 更新数据
UPDATE users
SET age = 26, updated_at = CURRENT_TIMESTAMP
WHERE username = 'alice';

-- 更新数据并返回
UPDATE users
SET age = age + 1
WHERE age < 30
RETURNING username, age;

-- 删除数据
DELETE FROM users
WHERE username = 'bob';

-- 删除所有数据（谨慎使用）
DELETE FROM users;

-- 删除数据并返回
DELETE FROM users
WHERE age > 50
RETURNING username, age;
```

### 3.4 查询操作

```sql
-- 基本查询
SELECT * FROM users;

-- 选择特定列
SELECT username, email, age FROM users;

-- 条件查询
SELECT * FROM users
WHERE age > 25 AND age < 40;

-- 模糊查询
SELECT * FROM users
WHERE username LIKE 'a%';

-- IN 查询
SELECT * FROM users
WHERE age IN (25, 30, 35);

-- BETWEEN 查询
SELECT * FROM users
WHERE age BETWEEN 20 AND 40;

-- IS NULL 查询
SELECT * FROM users
WHERE phone IS NULL;

-- 排序
SELECT * FROM users
ORDER BY age ASC;

SELECT * FROM users
ORDER BY age DESC, username ASC;

-- 分页
SELECT * FROM users
ORDER BY id
LIMIT 10 OFFSET 0;

-- 去重
SELECT DISTINCT age FROM users;

-- 列别名
SELECT username AS "用户名", email AS "邮箱" FROM users;

-- 连接查询
SELECT users.username, orders.order_number, orders.total_amount
FROM users
INNER JOIN orders ON users.id = orders.user_id;

-- 左连接
SELECT users.username, orders.order_number
FROM users
LEFT JOIN orders ON users.id = orders.user_id;

-- 聚合查询
SELECT 
    COUNT(*) AS total_users,
    AVG(age) AS avg_age,
    MAX(age) AS max_age,
    MIN(age) AS min_age
FROM users;

-- 分组查询
SELECT age, COUNT(*) AS count
FROM users
GROUP BY age
HAVING COUNT(*) > 1;

-- 子查询
SELECT * FROM users
WHERE age > (SELECT AVG(age) FROM users);

-- EXISTS 子查询
SELECT * FROM users
WHERE EXISTS (
    SELECT 1 FROM orders 
    WHERE orders.user_id = users.id
);
```

### 3.5 索引操作

```sql
-- 创建索引
CREATE INDEX idx_users_username ON users (username);

-- 创建唯一索引
CREATE UNIQUE INDEX idx_users_email ON users (email);

-- 创建复合索引
CREATE INDEX idx_users_age_username ON users (age, username);

-- 创建部分索引
CREATE INDEX idx_users_active ON users (username)
WHERE active = true;

-- 创建表达式索引
CREATE INDEX idx_users_lower_username ON users (LOWER(username));

-- 删除索引
DROP INDEX idx_users_username;

-- 查看索引
\d users

-- 分析查询（使用 EXPLAIN）
EXPLAIN SELECT * FROM users WHERE username = 'alice';

-- 分析并执行查询
EXPLAIN ANALYZE SELECT * FROM users WHERE username = 'alice';
```

### 3.6 事务操作

```sql
-- 开始事务
BEGIN;
-- 或
START TRANSACTION;

-- 执行多个操作
INSERT INTO users (username, email, password, age)
VALUES ('testuser', 'test@example.com', 'pass', 20);

UPDATE accounts SET balance = balance - 100 WHERE user_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE user_id = 2;

-- 提交事务
COMMIT;

-- 回滚事务
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE user_id = 1;
-- 发现错误，回滚
ROLLBACK;

-- 保存点
BEGIN;
INSERT INTO users (username, email, password, age)
VALUES ('user1', 'user1@example.com', 'pass', 20);

SAVEPOINT savepoint1;

INSERT INTO users (username, email, password, age)
VALUES ('user2', 'user2@example.com', 'pass', 25);

-- 回滚到保存点
ROLLBACK TO SAVEPOINT savepoint1;

COMMIT;

-- 设置事务隔离级别
SET TRANSACTION ISOLATION LEVEL READ COMMITTED;
BEGIN;
-- 事务操作
COMMIT;
```

### 3.7 视图操作

```sql
-- 创建视图
CREATE VIEW active_users AS
SELECT id, username, email, age
FROM users
WHERE active = true;

-- 创建视图并指定列名
CREATE VIEW user_summary (user_id, user_name, user_email) AS
SELECT id, username, email
FROM users;

-- 查询视图
SELECT * FROM active_users;

-- 创建可更新视图
CREATE VIEW young_users AS
SELECT id, username, age
FROM users
WHERE age < 30
WITH CHECK OPTION;

-- 通过视图更新
UPDATE young_users SET age = 25 WHERE id = 1;

-- 删除视图
DROP VIEW active_users;

-- 创建物化视图
CREATE MATERIALIZED VIEW user_stats AS
SELECT age, COUNT(*) AS count
FROM users
GROUP BY age;

-- 刷新物化视图
REFRESH MATERIALIZED VIEW user_stats;

-- 删除物化视图
DROP MATERIALIZED VIEW user_stats;
```

### 3.8 函数操作

```sql
-- 创建简单函数
CREATE OR REPLACE FUNCTION get_user_count()
RETURNS INTEGER AS $$
DECLARE
    count INTEGER;
BEGIN
    SELECT COUNT(*) INTO count FROM users;
    RETURN count;
END;
$$ LANGUAGE plpgsql;

-- 调用函数
SELECT get_user_count();

-- 创建带参数的函数
CREATE OR REPLACE FUNCTION get_user_by_id(p_id INTEGER)
RETURNS TABLE (
    id INTEGER,
    username VARCHAR(50),
    email VARCHAR(100),
    age INTEGER
) AS $$
BEGIN
    RETURN QUERY
    SELECT id, username, email, age
    FROM users
    WHERE id = p_id;
END;
$$ LANGUAGE plpgsql;

-- 调用带参数的函数
SELECT * FROM get_user_by_id(1);

-- 删除函数
DROP FUNCTION get_user_count();
DROP FUNCTION get_user_by_id(INTEGER);
```

### 3.9 序列操作

```sql
-- 创建序列
CREATE SEQUENCE user_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

-- 使用序列
SELECT nextval('user_id_seq'); -- 获取下一个值
SELECT currval('user_id_seq'); -- 获取当前值
SELECT lastval(); -- 获取最后一个序列值

-- 修改序列
ALTER SEQUENCE user_id_seq
    RESTART WITH 100
    INCREMENT BY 2;

-- 删除序列
DROP SEQUENCE user_id_seq;

-- SERIAL 类型（自动创建序列）
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50)
);
```

### 3.10 用户和权限

```sql
-- 创建用户
CREATE USER myuser WITH PASSWORD 'mypassword';

-- 创建用户并指定权限
CREATE USER myuser WITH PASSWORD 'mypassword'
    CREATEDB
    CREATEROLE;

-- 修改用户密码
ALTER USER myuser WITH PASSWORD 'newpassword';

-- 删除用户
DROP USER myuser;

-- 授予权限
GRANT SELECT, INSERT, UPDATE, DELETE ON users TO myuser;

-- 授予所有权限
GRANT ALL PRIVILEGES ON users TO myuser;

-- 授予模式权限
GRANT USAGE ON SCHEMA public TO myuser;

-- 授予表的权限
GRANT SELECT ON ALL TABLES IN SCHEMA public TO myuser;

-- 撤销权限
REVOKE INSERT, UPDATE, DELETE ON users FROM myuser;

-- 查看用户
\du

-- 查看权限
\dp users
```

---

## 4. 官方示例代码（可运行）

### 4.1 博客系统

```sql
-- 创建数据库
CREATE DATABASE blog;
\c blog;

-- 用户表
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    bio TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 文章表
CREATE TABLE posts (
    id SERIAL PRIMARY KEY,
    title VARCHAR(200) NOT NULL,
    content TEXT NOT NULL,
    author_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    views INTEGER DEFAULT 0,
    likes INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 评论表
CREATE TABLE comments (
    id SERIAL PRIMARY KEY,
    post_id INTEGER NOT NULL REFERENCES posts(id) ON DELETE CASCADE,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content TEXT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 标签表
CREATE TABLE tags (
    id SERIAL PRIMARY KEY,
    name VARCHAR(50) NOT NULL UNIQUE
);

-- 文章标签关联表
CREATE TABLE post_tags (
    post_id INTEGER REFERENCES posts(id) ON DELETE CASCADE,
    tag_id INTEGER REFERENCES tags(id) ON DELETE CASCADE,
    PRIMARY KEY (post_id, tag_id)
);

-- 创建索引
CREATE INDEX idx_posts_author_id ON posts(author_id);
CREATE INDEX idx_posts_created_at ON posts(created_at DESC);
CREATE INDEX idx_comments_post_id ON comments(post_id);
CREATE INDEX idx_comments_user_id ON comments(user_id);
CREATE INDEX idx_post_tags_post_id ON post_tags(post_id);
CREATE INDEX idx_post_tags_tag_id ON post_tags(tag_id);

-- 插入测试数据
INSERT INTO users (username, email, password_hash, bio) VALUES
('alice', 'alice@example.com', 'hashed_password', '技术爱好者'),
('bob', 'bob@example.com', 'hashed_password', '程序员');

INSERT INTO tags (name) VALUES
('PostgreSQL'),
('Database'),
('Programming'),
('Blog');

INSERT INTO posts (title, content, author_id) VALUES
('PostgreSQL 入门', '这是一篇关于 PostgreSQL 入门的文章...', 1),
('数据库设计最佳实践', '数据库设计的最佳实践...', 2);

INSERT INTO post_tags (post_id, tag_id) VALUES
(1, 1), (1, 2), (1, 4),
(2, 2), (2, 3);

INSERT INTO comments (post_id, user_id, content) VALUES
(1, 2, '很好的文章！'),
(1, 1, '谢谢支持！');

-- 查询文章及其作者
SELECT 
    p.id,
    p.title,
    p.content,
    p.views,
    p.likes,
    p.created_at,
    u.username AS author_name
FROM posts p
JOIN users u ON p.author_id = u.id
ORDER BY p.created_at DESC;

-- 查询文章及其标签
SELECT 
    p.id,
    p.title,
    ARRAY_AGG(t.name) AS tags
FROM posts p
LEFT JOIN post_tags pt ON p.id = pt.post_id
LEFT JOIN tags t ON pt.tag_id = t.id
GROUP BY p.id, p.title
ORDER BY p.created_at DESC;

-- 查询文章评论数
SELECT 
    p.id,
    p.title,
    COUNT(c.id) AS comment_count
FROM posts p
LEFT JOIN comments c ON p.id = c.post_id
GROUP BY p.id, p.title
ORDER BY comment_count DESC;

-- 增加文章浏览量
UPDATE posts
SET views = views + 1
WHERE id = 1
RETURNING views;

-- 创建视图：热门文章
CREATE VIEW popular_posts AS
SELECT 
    p.id,
    p.title,
    p.views,
    p.likes,
    u.username AS author_name
FROM posts p
JOIN users u ON p.author_id = u.id
ORDER BY p.views DESC
LIMIT 10;

-- 查询热门文章
SELECT * FROM popular_posts;

-- 创建函数：获取用户文章数
CREATE OR REPLACE FUNCTION get_user_post_count(p_user_id INTEGER)
RETURNS INTEGER AS $$
DECLARE
    count INTEGER;
BEGIN
    SELECT COUNT(*) INTO count FROM posts WHERE author_id = p_user_id;
    RETURN count;
END;
$$ LANGUAGE plpgsql;

-- 使用函数
SELECT username, get_user_post_count(id) AS post_count FROM users;

-- 创建触发器：更新文章更新时间
CREATE OR REPLACE FUNCTION update_updated_at_column()
RETURNS TRIGGER AS $$
BEGIN
    NEW.updated_at = CURRENT_TIMESTAMP;
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;

CREATE TRIGGER update_posts_updated_at
    BEFORE UPDATE ON posts
    FOR EACH ROW
    EXECUTE FUNCTION update_updated_at_column();

CREATE TRIGGER update_users_updated_at
    BEFORE UPDATE ON users
    FOR EACH ROW
    EXECUTE FUNCTION update_updated_at_column();
```

### 4.2 电商系统

```sql
-- 创建数据库
CREATE DATABASE ecommerce;
\c ecommerce;

-- 用户表
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    phone VARCHAR(20),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- 产品表
CREATE TABLE products (
    id SERIAL PRIMARY KEY,
    name VARCHAR(200) NOT NULL,
    description TEXT,
    price NUMERIC(10, 2) NOT NULL,
    stock INTEGER NOT NULL DEFAULT 0,
    category VARCHAR(100),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 订单表
CREATE TABLE orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id),
    order_number VARCHAR(50) NOT NULL UNIQUE,
    total_amount NUMERIC(10, 2) NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'pending',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP
);

-- 订单详情表
CREATE TABLE order_items (
    id SERIAL PRIMARY KEY,
    order_id INTEGER NOT NULL REFERENCES orders(id) ON DELETE CASCADE,
    product_id INTEGER NOT NULL REFERENCES products(id),
    quantity INTEGER NOT NULL,
    price NUMERIC(10, 2) NOT NULL,
    subtotal NUMERIC(10, 2) NOT NULL
);

-- 地址表
CREATE TABLE addresses (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    full_name VARCHAR(100) NOT NULL,
    phone VARCHAR(20) NOT NULL,
    address_line1 VARCHAR(255) NOT NULL,
    address_line2 VARCHAR(255),
    city VARCHAR(100) NOT NULL,
    postal_code VARCHAR(20),
    country VARCHAR(100) NOT NULL,
    is_default BOOLEAN DEFAULT false
);

-- 创建索引
CREATE INDEX idx_orders_user_id ON orders(user_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_orders_created_at ON orders(created_at DESC);
CREATE INDEX idx_order_items_order_id ON order_items(order_id);
CREATE INDEX idx_order_items_product_id ON order_items(product_id);
CREATE INDEX idx_products_category ON products(category);
CREATE INDEX idx_addresses_user_id ON addresses(user_id);

-- 插入测试数据
INSERT INTO products (name, description, price, stock, category) VALUES
('iPhone 15', '最新款苹果手机', 7999.00, 100, '手机'),
('MacBook Pro', '专业笔记本电脑', 14999.00, 50, '电脑'),
('AirPods Pro', '无线耳机', 1999.00, 200, '配件');

-- 创建函数：生成订单号
CREATE OR REPLACE FUNCTION generate_order_number()
RETURNS VARCHAR(50) AS $$
DECLARE
    order_date VARCHAR(8);
    order_seq INTEGER;
BEGIN
    order_date := TO_CHAR(CURRENT_DATE, 'YYYYMMDD');
    order_seq := NEXTVAL('order_number_seq');
    RETURN 'ORD' || order_date || LPAD(order_seq::TEXT, 6, '0');
END;
$$ LANGUAGE plpgsql;

CREATE SEQUENCE order_number_seq START WITH 1 INCREMENT BY 1;

-- 创建函数：创建订单
CREATE OR REPLACE FUNCTION create_order(
    p_user_id INTEGER,
    p_product_id INTEGER,
    p_quantity INTEGER
)
RETURNS INTEGER AS $$
DECLARE
    v_product RECORD;
    v_order_id INTEGER;
    v_order_number VARCHAR(50);
    v_total NUMERIC(10, 2);
BEGIN
    -- 获取产品信息
    SELECT * INTO v_product FROM products WHERE id = p_product_id;
    
    -- 检查库存
    IF v_product.stock < p_quantity THEN
        RAISE EXCEPTION '库存不足';
    END IF;
    
    -- 扣减库存
    UPDATE products
    SET stock = stock - p_quantity
    WHERE id = p_product_id;
    
    -- 计算总价
    v_total := v_product.price * p_quantity;
    
    -- 生成订单号
    v_order_number := generate_order_number();
    
    -- 创建订单
    INSERT INTO orders (user_id, order_number, total_amount, status)
    VALUES (p_user_id, v_order_number, v_total, 'pending')
    RETURNING id INTO v_order_id;
    
    -- 创建订单详情
    INSERT INTO order_items (order_id, product_id, quantity, price, subtotal)
    VALUES (v_order_id, p_product_id, p_quantity, v_product.price, v_total);
    
    RETURN v_order_id;
END;
$$ LANGUAGE plpgsql;

-- 使用函数创建订单
-- SELECT create_order(1, 1, 1);

-- 查询订单详情
SELECT 
    o.id,
    o.order_number,
    o.total_amount,
    o.status,
    o.created_at,
    u.username,
    u.email,
    ARRAY_AGG(
        json_build_object(
            'product_name', p.name,
            'quantity', oi.quantity,
            'price', oi.price,
            'subtotal', oi.subtotal
        )
    ) AS items
FROM orders o
JOIN users u ON o.user_id = u.id
JOIN order_items oi ON o.id = oi.order_id
JOIN products p ON oi.product_id = p.id
WHERE o.id = 1
GROUP BY o.id, u.id;

-- 销售统计
SELECT 
    DATE(o.created_at) AS date,
    COUNT(*) AS order_count,
    SUM(o.total_amount) AS total_revenue
FROM orders o
WHERE o.status != 'cancelled'
GROUP BY DATE(o.created_at)
ORDER BY date DESC;

-- 产品销售统计
SELECT 
    p.id,
    p.name,
    SUM(oi.quantity) AS total_quantity,
    SUM(oi.subtotal) AS total_revenue
FROM products p
LEFT JOIN order_items oi ON p.id = oi.product_id
GROUP BY p.id, p.name
ORDER BY total_revenue DESC NULLS LAST;
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 数据库设计

1. **规范化设计**
   - 遵循第三范式
   - 适当反规范化提高性能
   - 使用合适的数据类型

2. **主键设计**
   - 使用 SERIAL 或 UUID
   - 避免使用业务字段作为主键
   - 复合主键慎重使用

3. **索引设计**
   - 为 WHERE、JOIN、ORDER BY 字段创建索引
   - 避免过多索引
   - 定期分析索引使用情况
   - 考虑部分索引和表达式索引

### 5.2 查询优化

1. **使用 EXPLAIN**
   - 分析查询执行计划
   - 检查是否使用索引
   - 优化慢查询

2. **避免 SELECT ***
   - 只查询需要的字段
   - 减少数据传输
   - 提高查询效率

3. **合理使用连接**
   - 选择合适的连接类型
   - 避免过度连接
   - 使用子查询替代复杂连接

4. **分页查询**
   - 使用 LIMIT 和 OFFSET
   - 大分页使用键集分页
   - 避免深度分页

### 5.3 性能优化

1. **配置优化**
   - 合理设置 shared_buffers
   - 配置 work_mem
   - 调整 maintenance_work_mem
   - 配置有效缓存大小

2. **VACUUM 和 ANALYZE**
   - 定期运行 VACUUM
   - 定期运行 ANALYZE
   - 配置 autovacuum
   - 监控表膨胀

3. **事务优化**
   - 保持事务简短
   - 合理选择隔离级别
   - 避免长事务
   - 使用保存点

### 5.4 安全实践

1. **用户权限**
   - 最小权限原则
   - 定期审查权限
   - 移除不必要的权限

2. **密码安全**
   - 使用强密码
   - 定期更换密码
   - 不要在代码中硬编码密码

3. **数据加密**
   - 加密敏感数据
   - 使用 SSL/TLS 连接
   - 考虑透明数据加密

4. **备份恢复**
   - 定期备份
   - 测试恢复流程
   - 异地备份

### 5.5 运维最佳实践

1. **监控**
   - 监控慢查询
   - 监控连接数
   - 监控磁盘空间
   - 监控复制延迟

2. **备份策略**
   - 全量备份
   - 增量备份
   -  WAL 归档
   - Point-in-Time Recovery

3. **升级维护**
   - 测试环境先验证
   - 有回滚计划
   - 选择低峰期升级

---

## 6. 官方文档参考链接

- [PostgreSQL 官方文档](https://www.postgresql.org/docs/)
- [PostgreSQL 教程](https://www.postgresql.org/docs/current/tutorial.html)
- [SQL 语言](https://www.postgresql.org/docs/current/sql.html)
- [PL/pgSQL 手册](https://www.postgresql.org/docs/current/plpgsql.html)
- [PostgreSQL 性能优化](https://www.postgresql.org/docs/current/performance-tips.html)
- [PostgreSQL 管理指南](https://www.postgresql.org/docs/current/admin.html)
- [PostgreSQL 最佳实践](https://wiki.postgresql.org/wiki/Performance_Optimization)
