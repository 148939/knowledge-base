# MongoDB 基础入门

## 1. 核心概述（官方定义）

MongoDB 是一个基于分布式文件存储的开源 NoSQL 数据库系统，由 C++ 语言编写。它是一个文档数据库，具有高性能、高可用性和易扩展性，能够存储 JSON 风格的文档数据。

### 核心概念
- **Database**：数据库，包含多个集合
- **Collection**：集合，类似于关系数据库的表
- **Document**：文档，类似于关系数据库的行（JSON/BSON 格式）
- **Field**：字段，文档中的键值对
- **Index**：索引，提高查询性能
- **Replica Set**：副本集，提供高可用
- **Sharding**：分片，水平扩展

---

## 2. 核心知识点（结构化层级）

### 2.1 数据模型

#### BSON 文档
- 二进制 JSON 格式
- 支持更多数据类型
- 文档大小限制 16MB

#### 数据类型
- String：字符串
- Integer：整数
- Boolean：布尔值
- Double：双精度浮点数
- ObjectId：文档 ID
- Date：日期时间
- Timestamp：时间戳
- Array：数组
- Object：嵌套文档
- Null：空值

#### 数据建模
- 嵌入式文档（Embed）
- 引用式文档（Reference）
- 混合模式

### 2.2 基本操作

#### 数据库操作
- 显示数据库
- 创建数据库
- 删除数据库
- 切换数据库

#### 集合操作
- 显示集合
- 创建集合
- 删除集合
- 重命名集合

#### 文档操作
- 插入文档
- 查询文档
- 更新文档
- 删除文档
- 批量操作

### 2.3 查询操作

#### 基本查询
- find()：查询文档
- findOne()：查询单个文档
- 投影（Projection）：选择字段

#### 条件查询
- 比较操作符（$eq, $gt, $lt, $gte, $lte, $ne）
- 逻辑操作符（$and, $or, $not, $nor）
- 元素操作符（$exists, $type）
- 数组操作符（$in, $nin, $all, $size）

#### 排序与分页
- sort()：排序
- skip()：跳过文档
- limit()：限制数量

### 2.4 更新操作

#### 基本更新
- updateOne()：更新单个文档
- updateMany()：更新多个文档
- replaceOne()：替换文档

#### 更新操作符
- $set：设置字段值
- $unset：删除字段
- $inc：增减数值
- $push：添加数组元素
- $pull：删除数组元素
- $addToSet：添加不重复元素

### 2.5 删除操作

#### 删除方法
- deleteOne()：删除单个文档
- deleteMany()：删除多个文档
- drop()：删除集合

#### 删除注意事项
- 事务支持
- 删除前备份
- 小心删除所有数据

### 2.6 索引

#### 索引类型
- 单字段索引
- 复合索引
- 多键索引
- 文本索引
- 地理空间索引
- 哈希索引

#### 索引操作
- 创建索引
- 删除索引
- 查看索引
- 索引使用分析

### 2.7 聚合框架

#### 聚合管道
- $match：过滤文档
- $project：投影字段
- $group：分组聚合
- $sort：排序
- $limit：限制数量
- $skip：跳过文档
- $unwind：展开数组
- $lookup：关联查询

#### 聚合表达式
- 算术表达式
- 字符串表达式
- 日期表达式
- 数组表达式
- 条件表达式

### 2.8 副本集

#### 副本集概念
- 主节点（Primary）
- 从节点（Secondary）
- 仲裁者（Arbiter）

#### 副本集操作
- 初始化副本集
- 添加节点
- 删除节点
- 查看副本集状态

### 2.9 分片

#### 分片概念
- 分片键（Shard Key）
- 分片服务器（Shard）
- 配置服务器（Config Server）
- 路由服务器（Mongos）

#### 分片策略
- 范围分片（Range Sharding）
- 哈希分片（Hash Sharding）
- 区域分片（Zone Sharding）

---

## 3. 官方标准语法 / API

### 3.1 数据库操作

```javascript
// 显示所有数据库
show dbs;

// 切换到指定数据库（不存在则创建）
use mydb;

// 显示当前数据库
db;

// 删除当前数据库
db.dropDatabase();
```

### 3.2 集合操作

```javascript
// 显示所有集合
show collections;

// 创建集合
db.createCollection("users");

// 带选项创建集合
db.createCollection("users", {
    capped: true,
    size: 10000,
    max: 100
});

// 删除集合
db.users.drop();

// 重命名集合
db.users.renameCollection("customers");
```

### 3.3 文档操作

```javascript
// 插入单个文档
db.users.insertOne({
    name: "Alice",
    age: 30,
    email: "alice@example.com",
    hobbies: ["reading", "hiking"],
    address: {
        city: "New York",
        country: "USA"
    }
});

// 插入多个文档
db.users.insertMany([
    { name: "Bob", age: 25 },
    { name: "Charlie", age: 35 }
]);

// 查询所有文档
db.users.find();

// 查询单个文档
db.users.findOne({ name: "Alice" });

// 更新单个文档
db.users.updateOne(
    { name: "Alice" },
    { $set: { age: 31 } }
);

// 更新多个文档
db.users.updateMany(
    { age: { $lt: 30 } },
    { $set: { status: "young" } }
);

// 删除单个文档
db.users.deleteOne({ name: "Bob" });

// 删除多个文档
db.users.deleteMany({ age: { $gt: 30 } });
```

### 3.4 查询操作

```javascript
// 基本查询
db.users.find({ age: 30 });

// 投影查询（只返回指定字段）
db.users.find(
    { age: { $gt: 25 } },
    { name: 1, email: 1, _id: 0 }
);

// 比较操作符
db.users.find({ age: { $gt: 25, $lt: 40 } });

// 逻辑操作符
db.users.find({
    $or: [
        { age: { $lt: 25 } },
        { age: { $gt: 40 } }
    ]
});

// 数组查询
db.users.find({ hobbies: "reading" });
db.users.find({ hobbies: { $in: ["reading", "swimming"] } });

// 嵌套文档查询
db.users.find({ "address.city": "New York" });

// 排序
db.users.find().sort({ age: 1 }); // 升序
db.users.find().sort({ age: -1 }); // 降序

// 分页
db.users.find().skip(10).limit(5);

// 计数
db.users.countDocuments({ age: { $gt: 25 } });

// 去重
db.users.distinct("hobbies");
```

### 3.5 更新操作

```javascript
// 设置字段
db.users.updateOne(
    { name: "Alice" },
    { $set: { 
        age: 31,
        "address.city": "London"
    } }
);

// 删除字段
db.users.updateOne(
    { name: "Alice" },
    { $unset: { oldField: "" } }
);

// 增减数值
db.users.updateOne(
    { name: "Alice" },
    { $inc: { age: 1 } }
);

// 数组操作
db.users.updateOne(
    { name: "Alice" },
    { $push: { hobbies: "swimming" } }
);

db.users.updateOne(
    { name: "Alice" },
    { $addToSet: { hobbies: "reading" } } // 不重复添加
);

db.users.updateOne(
    { name: "Alice" },
    { $pull: { hobbies: "hiking" } }
);

// 更新多个文档
db.users.updateMany(
    { status: "new" },
    { $set: { status: "active" } }
);

// 替换文档
db.users.replaceOne(
    { name: "Alice" },
    { name: "Alice", age: 32, status: "active" }
);
```

### 3.6 索引操作

```javascript
// 创建单字段索引
db.users.createIndex({ name: 1 });

// 创建复合索引
db.users.createIndex({ name: 1, age: -1 });

// 创建唯一索引
db.users.createIndex({ email: 1 }, { unique: true });

// 创建文本索引
db.users.createIndex({ name: "text", bio: "text" });

// 查看索引
db.users.getIndexes();

// 查看索引大小
db.users.totalIndexSize();

// 删除索引
db.users.dropIndex("name_1");

// 删除所有索引
db.users.dropIndexes();

// 使用 explain 分析查询
db.users.find({ name: "Alice" }).explain();
```

### 3.7 聚合操作

```javascript
// 简单聚合
db.users.aggregate([
    { $match: { age: { $gte: 25 } } },
    { $group: { 
        _id: null, 
        avgAge: { $avg: "$age" },
        total: { $sum: 1 }
    } }
]);

// 分组聚合
db.users.aggregate([
    { $group: {
        _id: "$address.city",
        count: { $sum: 1 },
        avgAge: { $avg: "$age" }
    } }
]);

// 排序和分页
db.users.aggregate([
    { $match: { age: { $gte: 25 } } },
    { $sort: { age: 1 } },
    { $skip: 0 },
    { $limit: 10 }
]);

// 展开数组
db.users.aggregate([
    { $unwind: "$hobbies" },
    { $group: {
        _id: "$hobbies",
        count: { $sum: 1 }
    } }
]);

// 关联查询（$lookup）
db.orders.aggregate([
    {
        $lookup: {
            from: "users",
            localField: "userId",
            foreignField: "_id",
            as: "user"
        }
    }
]);

// 投影字段
db.users.aggregate([
    {
        $project: {
            name: 1,
            age: 1,
            email: 1,
            isYoung: { $lt: ["$age", 30] }
        }
    }
]);
```

### 3.8 Python 客户端（PyMongo）

```python
from pymongo import MongoClient
from bson.objectid import ObjectId
import datetime

# 连接数据库
client = MongoClient("mongodb://localhost:27017/")
db = client["mydb"]
users = db["users"]

# 插入文档
user = {
    "name": "Alice",
    "age": 30,
    "email": "alice@example.com",
    "created_at": datetime.datetime.utcnow()
}
result = users.insert_one(user)
print(f"Inserted ID: {result.inserted_id}")

# 插入多个文档
users_list = [
    {"name": "Bob", "age": 25},
    {"name": "Charlie", "age": 35}
]
results = users.insert_many(users_list)
print(f"Inserted IDs: {results.inserted_ids}")

# 查询文档
user = users.find_one({"name": "Alice"})
print(f"Found user: {user}")

# 查询多个文档
for user in users.find({"age": {"$gt": 25}}):
    print(user)

# 更新文档
result = users.update_one(
    {"name": "Alice"},
    {"$set": {"age": 31}}
)
print(f"Modified count: {result.modified_count}")

# 删除文档
result = users.delete_one({"name": "Bob"})
print(f"Deleted count: {result.deleted_count}")

# 聚合
pipeline = [
    {"$match": {"age": {"$gte": 25}}},
    {"$group": {"_id": None, "avg_age": {"$avg": "$age"}}}
]
result = list(users.aggregate(pipeline))
print(f"Average age: {result[0]['avg_age']}")
```

---

## 4. 官方示例代码（可运行）

### 4.1 博客系统

```javascript
// 初始化数据
use blog;

// 用户集合
db.createCollection("users");
db.users.insertMany([
    {
        _id: ObjectId("60f1a0b3c9e77b001f8e4d1a"),
        username: "alice",
        email: "alice@example.com",
        password: "hashed_password",
        created_at: ISODate("2024-01-01T00:00:00Z")
    },
    {
        _id: ObjectId("60f1a0b3c9e77b001f8e4d1b"),
        username: "bob",
        email: "bob@example.com",
        password: "hashed_password",
        created_at: ISODate("2024-01-02T00:00:00Z")
    }
]);

// 文章集合
db.createCollection("posts");
db.posts.insertMany([
    {
        title: "MongoDB 入门指南",
        content: "MongoDB 是一个强大的文档数据库...",
        author_id: ObjectId("60f1a0b3c9e77b001f8e4d1a"),
        tags: ["MongoDB", "Database", "NoSQL"],
        views: 1000,
        likes: 50,
        comments: [
            {
                user_id: ObjectId("60f1a0b3c9e77b001f8e4d1b"),
                content: "很好的文章！",
                created_at: ISODate("2024-01-05T00:00:00Z")
            }
        ],
        created_at: ISODate("2024-01-03T00:00:00Z"),
        updated_at: ISODate("2024-01-04T00:00:00Z")
    }
]);

// 查询文章及其作者
db.posts.aggregate([
    {
        $lookup: {
            from: "users",
            localField: "author_id",
            foreignField: "_id",
            as: "author"
        }
    },
    { $unwind: "$author" },
    {
        $project: {
            title: 1,
            content: 1,
            "author.username": 1,
            "author.email": 1,
            tags: 1,
            views: 1,
            created_at: 1
        }
    }
]);

// 查询热门文章
db.posts.find(
    {},
    { title: 1, views: 1, likes: 1 }
).sort({ views: -1 }).limit(5);

// 按标签统计文章数量
db.posts.aggregate([
    { $unwind: "$tags" },
    {
        $group: {
            _id: "$tags",
            count: { $sum: 1 }
        }
    },
    { $sort: { count: -1 } }
]);

// 增加文章浏览量
db.posts.updateOne(
    { _id: ObjectId("60f1a0b3c9e77b001f8e4d1c") },
    { $inc: { views: 1 } }
);

// 添加评论
db.posts.updateOne(
    { _id: ObjectId("60f1a0b3c9e77b001f8e4d1c") },
    {
        $push: {
            comments: {
                user_id: ObjectId("60f1a0b3c9e77b001f8e4d1a"),
                content: "新评论",
                created_at: ISODate()
            }
        }
    }
);

// 创建索引
db.posts.createIndex({ author_id: 1 });
db.posts.createIndex({ tags: 1 });
db.posts.createIndex({ created_at: -1 });
db.posts.createIndex({ title: "text", content: "text" });
```

### 4.2 电商系统

```python
from pymongo import MongoClient
from bson.objectid import ObjectId
import datetime

# 连接数据库
client = MongoClient("mongodb://localhost:27017/")
db = client["ecommerce"]

# 初始化集合
products = db["products"]
orders = db["orders"]
users = db["users"]

# 产品集合
products.insert_many([
    {
        "name": "iPhone 15",
        "description": "最新款苹果手机",
        "price": 7999.00,
        "stock": 100,
        "category": "手机",
        "tags": ["Apple", "iPhone", "智能手机"],
        "created_at": datetime.datetime.utcnow()
    },
    {
        "name": "MacBook Pro",
        "description": "专业笔记本电脑",
        "price": 14999.00,
        "stock": 50,
        "category": "电脑",
        "tags": ["Apple", "MacBook", "笔记本"],
        "created_at": datetime.datetime.utcnow()
    }
])

# 创建产品索引
products.create_index("category")
products.create_index("tags")
products.create_index([("price", 1)])

# 查询产品
def get_product_by_id(product_id):
    return products.find_one({"_id": ObjectId(product_id)})

def get_products_by_category(category, page=1, page_size=10):
    skip = (page - 1) * page_size
    return list(products.find({"category": category})
                  .skip(skip)
                  .limit(page_size))

def search_products(keyword):
    return list(products.find({
        "$or": [
            {"name": {"$regex": keyword, "$options": "i"}},
            {"description": {"$regex": keyword, "$options": "i"}},
            {"tags": {"$in": [keyword]}}
        ]
    }))

# 更新库存
def update_stock(product_id, quantity):
    result = products.update_one(
        {"_id": ObjectId(product_id)},
        {"$inc": {"stock": -quantity}}
    )
    return result.modified_count > 0

# 创建订单
def create_order(user_id, items):
    order = {
        "user_id": ObjectId(user_id),
        "items": items,
        "total_amount": sum(item["price"] * item["quantity"] for item in items),
        "status": "pending",
        "created_at": datetime.datetime.utcnow()
    }
    
    # 检查库存
    for item in items:
        product = products.find_one({"_id": ObjectId(item["product_id"])})
        if not product or product["stock"] < item["quantity"]:
            raise Exception(f"产品 {item['product_id']} 库存不足")
    
    # 扣减库存
    for item in items:
        update_stock(item["product_id"], item["quantity"])
    
    # 创建订单
    result = orders.insert_one(order)
    return result.inserted_id

# 查询订单
def get_order_by_id(order_id):
    return orders.find_one({"_id": ObjectId(order_id)})

def get_orders_by_user(user_id):
    return list(orders.find({"user_id": ObjectId(user_id)})
                 .sort({"created_at": -1}))

# 更新订单状态
def update_order_status(order_id, status):
    result = orders.update_one(
        {"_id": ObjectId(order_id)},
        {
            "$set": {"status": status},
            "$push": {"status_history": {
                "status": status,
                "time": datetime.datetime.utcnow()
            }}
        }
    )
    return result.modified_count > 0

# 销售统计
def get_sales_stats(start_date, end_date):
    pipeline = [
        {
            "$match": {
                "created_at": {
                    "$gte": start_date,
                    "$lte": end_date
                },
                "status": {"$ne": "cancelled"}
            }
        },
        {
            "$group": {
                "_id": None,
                "total_orders": {"$sum": 1},
                "total_revenue": {"$sum": "$total_amount"},
                "avg_order_amount": {"$avg": "$total_amount"}
            }
        }
    ]
    
    result = list(orders.aggregate(pipeline))
    return result[0] if result else None

# 使用示例
if __name__ == "__main__":
    # 查询产品
    print("查询手机分类产品:")
    for product in get_products_by_category("手机"):
        print(f"{product['name']} - ¥{product['price']}")
    
    # 创建订单
    print("\n创建订单:")
    try:
        product = products.find_one({"name": "iPhone 15"})
        order_items = [
            {"product_id": str(product["_id"]), "price": product["price"], "quantity": 1}
        ]
        
        user = users.insert_one({
            "username": "testuser",
            "email": "test@example.com"
        })
        
        order_id = create_order(str(user.inserted_id), order_items)
        print(f"订单创建成功: {order_id}")
    except Exception as e:
        print(f"订单创建失败: {e}")
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 数据建模

1. **嵌入式 vs 引用式**
   - 一对一关系：优先嵌入
   - 一对多关系：少则嵌入，多则引用
   - 多对多关系：使用引用
   - 考虑数据更新频率

2. **文档大小限制**
   - 单个文档不超过 16MB
   - 大数据考虑 GridFS
   - 合理拆分文档

3. **索引设计**
   - 为常用查询创建索引
   - 复合索引顺序很重要
   - 避免索引过多
   - 定期分析索引使用情况

### 5.2 查询优化

1. **使用索引**
   - 确保查询使用索引
   - 使用 explain() 分析
   - 避免全表扫描

2. **投影查询**
   - 只查询需要的字段
   - 减少网络传输
   - 提高查询效率

3. **分页查询**
   - 使用 skip() 和 limit()
   - 大分页使用范围查询
   - 避免深度分页

### 5.3 性能优化

1. **写入优化**
   - 批量插入使用 insertMany()
   - 合理使用 write concern
   - 考虑使用 bulkWrite()

2. **内存配置**
   - 确保工作集在内存中
   - 合理配置 WiredTiger 缓存
   - 监控内存使用

3. **分片策略**
   - 选择合适的分片键
   - 避免热点问题
   - 均匀分布数据

### 5.4 安全实践

1. **认证授权**
   - 启用身份验证
   - 使用最小权限原则
   - 定期更改密码

2. **网络安全**
   - 使用 TLS/SSL 加密
   - 限制网络访问
   - 使用 VPN 或专用网络

3. **数据安全**
   - 定期备份数据
   - 启用审计日志
   - 敏感数据加密

### 5.5 运维最佳实践

1. **监控**
   - 监控慢查询
   - 监控连接数
   - 监控复制延迟
   - 设置告警

2. **备份恢复**
   - 定期全量备份
   - 增量备份
   - 测试恢复流程

3. **升级维护**
   - 测试环境先验证
   - 滚动升级副本集
   - 有回滚计划

---

## 6. 官方文档参考链接

- [MongoDB 官方文档](https://www.mongodb.com/docs/)
- [MongoDB 入门指南](https://www.mongodb.com/docs/manual/tutorial/getting-started/)
- [MongoDB CRUD 操作](https://www.mongodb.com/docs/manual/crud/)
- [MongoDB 聚合框架](https://www.mongodb.com/docs/manual/aggregation/)
- [MongoDB 索引](https://www.mongodb.com/docs/manual/indexes/)
- [PyMongo 文档](https://pymongo.readthedocs.io/)
- [MongoDB 最佳实践](https://www.mongodb.com/docs/manual/core/best-practices/)
