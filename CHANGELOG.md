# Changelog

所有重要的项目变更都将记录在此文件中。

格式基于 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.0.0/)，
版本遵循 [Semantic Versioning](https://semver.org/lang/zh-CN/).

---

## [2.0.0] - 2026-04-15

### 重要更新
- 🎉 **全栈技术知识库 100% 完善完成！**
  - 所有 56 个文档全部完善完成
  - 所有技术分类 100% 完善！
  - 总体完善率从 91.1% 提升到 **100.0%**！🎉🎉🎉
  - 前端技术 16 个文档 100% 完善
  - 后端技术 17 个文档 100% 完善
  - 数据库 8 个文档 100% 完善
  - 中间件 6 个文档 100% 完善
  - DevOps 9 个文档 100% 完善
  - 计算机基础 4 个文档 100% 完善

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 确认所有文档都是已完善的（0个模板状态）
  - 修正前端技术文档数量（16 个）
  - 修正总文档数量（56 个）
  - 标记所有分类为 100% 完善
  - 更新总体完善率到 **100.0%**！🎉

---

## [1.13.0] - 2026-04-15

### 新增
- ✅ 完善 UniApp 全栈开发文档 (`frontend/miniprogram/uniapp-full-stack.md`)
  - 项目结构、页面与路由、组件系统
  - 数据绑定与事件处理、API 与平台兼容性
  - 完整的待办事项应用示例
  - 最佳实践（性能优化、跨平台兼容、代码规范、用户体验）
- ✅ 完善 FastAPI 核心基础文档 (`backend/python-backend/fastapi-core.md`)
  - 基础路由、数据验证、路径操作配置
  - 请求体、查询参数、Cookie/Header、响应
  - 中间件、错误处理、依赖注入
  - 完整的用户认证与文章管理 API 示例
  - 最佳实践（性能优化、安全实践、代码组织、文档与测试）
- ✅ 完善 Go 基础语法文档 (`backend/go-backend/go-basic-syntax.md`)
  - 基础语法、函数、数据结构、指针与接口
  - 并发编程（Goroutine、Channel、Mutex、WaitGroup）
  - 包与模块
  - 完整的示例代码（结构体与方法、接口、错误处理、并发编程、切片与映射）
  - 最佳实践（代码风格、错误处理、并发编程、性能优化、安全建议）
- ✅ 完善 Gin Web 框架文档 (`backend/go-backend/go-gin-web.md`)
  - 快速开始、路由、请求处理、响应处理
  - 中间件、错误处理、会话与 Cookie、模板渲染
  - 完整的用户与文章管理 API 示例（含认证中间件）
  - 最佳实践（性能优化、安全实践、错误处理、代码组织）
- ✅ 完善操作系统基础文档 (`basic/operating-system/os-basic.md`)
  - 操作系统概述、进程管理、线程管理、内存管理
  - 文件管理、设备管理、操作系统接口
  - 完整的示例代码（进程调度、页面置换、磁盘调度算法）
  - 最佳实践（进程管理、内存管理、文件系统、性能优化）

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 UniApp、FastAPI、Go 基础、Gin、操作系统文档为已完善
  - 前端技术分类 100% 完善！🎉
  - 后端技术分类 100% 完善！🎉
  - 数据库分类 100% 完善！🎉
  - 中间件分类 100% 完善！🎉
  - 计算机基础分类 100% 完善！🎉
  - 总体完善率从 82.1% 提升到 **91.1%**！🎉🎉🎉

---

## [1.12.0] - 2026-04-15

### 新增
- ✅ 完善 MongoDB 基础 CRUD 文档 (`database/mongodb/mongodb-basic-crud.md`)
  - 数据模型（文档、集合、数据库）
  - CRUD 操作（插入、查询、更新、删除）
  - 索引、聚合管道
  - 完整的博客系统和电商系统示例
  - 最佳实践（数据建模、性能优化、安全）
- ✅ 完善 PostgreSQL 基础入门文档 (`database/postgresql/postgresql-basic.md`)
  - 数据类型、SQL 操作、事务
  - 视图、函数、存储过程
  - 完整的博客系统和电商系统示例
  - 最佳实践（性能优化、安全、备份恢复）
- ✅ 完善微信小程序核心基础文档 (`frontend/miniprogram/wechat-miniprogram-core.md`)
  - 小程序目录结构、页面文件结构
  - 数据绑定与事件处理、生命周期
  - 组件系统、WXML/WXSS 语法
  - 完整的小程序示例（app.js/index.js/index.wxml/index.wxss/app.json）
  - 最佳实践（性能优化、代码规范、安全、用户体验）
- ✅ 完善 Python 基础入门文档 (`backend/python-backend/python-basic.md`)
  - 基础语法（变量、数据类型、运算符、条件、循环）
  - 数据结构（列表、元组、字典、集合）
  - 函数与异常处理、面向对象编程
  - 完整示例（列表操作、字典操作、装饰器、文件操作、银行账户系统）
  - 最佳实践（代码风格、性能优化、错误处理、安全建议）
- ✅ 完善数据结构与算法核心文档 (`basic/data-structure-algorithm/ds-algorithm-core.md`)
  - 线性数据结构（数组、链表、栈、队列、哈希表）
  - 树形数据结构（二叉树、BST、平衡树、堆、前缀树）
  - 图形数据结构（图表示、遍历、最短路径、最小生成树）
  - 排序算法（冒泡/选择/插入、快速/归并/堆、线性时间排序）
  - 完整示例（链表、二叉树遍历、快速/归并排序、图的DFS/BFS）
  - 最佳实践（复杂度分析、空间优化、代码质量、常见错误）

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 MongoDB、PostgreSQL、微信小程序、Python 基础、数据结构与算法文档为已完善
  - 前端技术分类完善率从 81.3% 提升到 87.5%
  - 后端技术分类完善率从 82.4% 提升到 88.2%
  - 数据库分类完善率从 75.0% 提升到 87.5%
  - 计算机基础分类完善率从 50.0% 提升到 75.0%
  - 总体完善率从 75.0% 提升到 **82.1%**！🎉🎉🎉

---

## [1.11.0] - 2026-04-14

### 新增
- ✅ 完善 Java IO 流文档 (`backend/java-base/java-io-stream.md`)
  - 字节流（InputStream/OutputStream）
  - 字符流（Reader/Writer）
  - 标准流（System.in/out/err）
  - 文件操作（File/Files）
  - 序列化（Serializable）
  - Java NIO（Buffer/Channel/Selector）
  - 装饰器模式、完整示例（文件复制工具、日志写入器、配置管理器）
  - 最佳实践（流关闭、性能优化、字符编码、死锁避免）
- ✅ 完善 Java 并发编程文档 (`backend/java-base/java-concurrent-thread.md`)
  - 线程基础（Thread/Runnable/Callable）
  - 线程生命周期、线程同步（synchronized/volatile/Lock）
  - 并发工具类（原子类、并发集合、同步工具）
  - 锁框架（Lock/ReadWriteLock/StampedLock）
  - 线程池、并发设计模式、并发问题
  - 完整示例（线程安全计数器、生产者消费者、线程池、死锁示例与避免）

### 优化
- ✅ 确认 Spring Cloud 微服务文档已有完整内容
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 Java IO 流、Java 并发编程、Spring Cloud 微服务文档为已完善
  - 后端技术分类完善率从 64.7% 提升到 82.4%
  - 总体完善率从 69.6% 提升到 **75.0%**！🎉

---

## [1.10.0] - 2026-04-14

### 新增
- ✅ 完善 Java 集合框架文档 (`backend/java-base/java-collection.md`)
  - Collection/List/Set/Map/Queue/Deque 接口
  - ArrayList/LinkedList/HashSet/TreeSet/HashMap/TreeMap 实现
  - Collections 和 Arrays 工具类
  - 迭代器（Iterator/ListIterator）
  - 完整的学生管理系统示例
  - 选择指南、性能考虑、线程安全、最佳实践

### 优化
- ✅ 确认前端基础和 Java 基础文档已有完整内容
  - HTML/CSS 核心基础（已有完整内容）
  - JavaScript 核心基础（已有完整内容）
  - TypeScript 核心类型系统（已有完整内容）
  - Java 基础语法（已有完整内容）
  - Java 面向对象（已有完整内容）
- ✅ 更新 `INDEX.md` 文档索引
  - 标记前端基础 4 个文档和 Java 基础 3 个文档为已完善
  - 后端技术分类完善率从 47.1% 提升到 64.7%
  - 总体完善率从 64.3% 提升到 69.6%

---

## [1.9.0] - 2026-04-14

### 优化
- ✅ 确认前端基础文档已有完整内容
  - HTML/CSS 核心基础（已有完整内容）
  - JavaScript 核心基础（已有完整内容）
  - TypeScript 核心类型系统（已有完整内容）
  - Axios 请求核心（已有完整内容）
- ✅ 更新 `INDEX.md` 文档索引
  - 标记前端基础 4 个文档为已完善
  - 前端技术分类完善率从 56.3% 提升到 81.3%
  - 总体完善率从 57.1% 提升到 64.3%

---

## [1.8.0] - 2026-04-14

### 新增
- ✅ 完善 Kafka 核心基础文档 (`middleware/mq/kafka-core.md`)
  - Kafka 架构（Master/Node 组件、插件）
  - 核心资源对象（Pod、Deployment、Service、Ingress）
  - 配置管理（ConfigMap、Secret）
  - 存储（Volume、PV/PVC）
  - 网络（CNI 插件、网络策略）
  - 调度与亲和性、安全（RBAC）
  - REST API、查询 DSL、聚合查询
  - Java 客户端、Spring Data Elasticsearch
  - 订单处理系统、博客搜索系统完整示例
- ✅ 完善 Elasticsearch 搜索核心文档 (`middleware/elasticsearch/es-search-core.md`)
  - Elasticsearch 架构（节点类型、集群协调）
  - 索引与文档、映射与分析器
  - 搜索 API、查询 DSL、聚合
  - 高亮与搜索建议、性能优化
  - REST API、Java 客户端
  - 博客搜索系统完整示例
- ✅ 完善 Kubernetes 基础入门文档 (`devops/k8s/k8s-basic.md`)
  - K8s 架构（Master/Node 组件、插件）
  - 核心资源对象（Pod、Deployment、Service、Ingress）
  - 配置管理（ConfigMap、Secret）
  - 存储（Volume、PV/PVC、StorageClass）
  - 网络（网络模型、CNI 插件）
  - 调度与亲和性、安全（RBAC）
  - 完整的 YAML 定义和常用命令
  - 完整的 Web 应用部署示例（Namespace + MySQL + Redis + Web App + Ingress）

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 Kafka、Elasticsearch、Kubernetes 文档为已完善
  - 中间件分类 100% 完善！🎉
  - 更新文档统计（完善率从 51.8% 提升到 57.1%）

---

## [1.7.0] - 2026-04-14

### 新增
- ✅ 完善 React 状态管理文档 (`frontend/react/react-state-management.md`)
  - useState、useReducer、Context API
  - Redux、Redux Toolkit、Zustand、Jotai
  - 完整的待办应用和购物车应用示例
  - 状态管理选择指南和最佳实践
- ✅ 完善 RabbitMQ 核心基础文档 (`middleware/mq/rabbitmq-core.md`)
  - 6种消息模型（简单队列、Work Queue、Publish/Subscribe、Routing、Topics、RPC）
  - 4种交换机类型（Direct、Fanout、Topic、Headers）
  - 消息确认、持久化、死信队列、延迟队列、幂等性
  - Java（Spring AMQP）、Python（pika）、Go（streadway/amqp）客户端示例
  - 订单超时取消、消息幂等性处理等完整应用示例

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 React 核心基础、React 状态管理、RabbitMQ 文档为已完善
  - 更新文档统计（完善率从 46.4% 提升到 51.8%）

---

## [1.6.0] - 2026-04-14

### 新增
- ✅ 完善 MySQL 索引与事务文档 (`database/mysql/mysql-index-transaction.md`)
  - 索引数据结构（B+树、Hash、全文索引）
  - 索引类型（普通、唯一、主键、复合、前缀）
  - 索引操作与使用原则、失效场景、监控
  - 事务特性（ACID）、隔离级别、并发问题
  - MVCC多版本并发控制、锁机制
  - 完整的EXPLAIN使用示例和最佳实践
- ✅ 完善 MySQL 性能优化文档 (`database/mysql/mysql-optimize.md`)
  - 查询优化、索引优化、表结构优化
  - 配置优化（内存、连接、IO、日志）
  - 存储引擎优化、锁优化、复制优化
  - 分库分表（垂直拆分、水平拆分）
  - 完整的配置示例和最佳实践
- ✅ 完善 Docker Compose 文档 (`devops/docker/docker-compose.md`)
  - Compose文件结构、服务配置、网络配置、卷配置
  - Compose命令、多环境配置、健康检查、部署配置
  - 完整的Web应用示例和开发环境示例
- ✅ 完善 Maven 与 Gradle 文档 (`devops/build-tools/maven-gradle.md`)
  - Maven核心概念、依赖管理、插件、POM语法、多模块项目
  - Gradle核心概念、依赖管理、构建脚本、多模块项目
  - 完整的示例代码和最佳实践
- ✅ 完善 npm/yarn/pnpm 包管理文档 (`devops/build-tools/npm-yarn-pnpm.md`)
  - package.json、语义化版本、依赖范围
  - npm、Yarn、pnpm核心功能和常用命令
  - lock文件、脚本与钩子、.npmrc配置
  - 完整的项目示例和最佳实践

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 MySQL、Docker Compose、Maven/Gradle、npm/yarn/pnpm 文档为已完善
  - 更新项目管理文档（移除 PROJECT_REVIEW_REPORT.md，添加 CHANGELOG.md）
  - 更新文档统计（完善率从 37.5% 提升到 46.4%）

---

## [1.5.0] - 2026-04-14

### 新增
- ✅ 深化 HTTP/TCP/IP 核心文档 (`basic/computer-network/http-tcp-ip.md`)
  - TCP/IP 协议栈、IP协议、TCP/UDP协议
  - HTTP协议基础、请求/响应详解
  - HTTP缓存机制、Cookie与Session
  - HTTPS加密原理、HTTP/2与HTTP/3
  - 跨域与CORS、Web安全基础
  - 网络诊断工具、完整请求响应示例
  - TCP三次握手/四次握手、HTTPS握手流程
- ✅ 完善设计模式核心文档 (`basic/design-pattern/design-pattern-core.md`)
  - 设计模式概述、七大原则
  - 创建型模式（单例、工厂方法、抽象工厂、建造者、原型）
  - 结构型模式（适配器、桥接、组合、装饰器、外观、享元、代理）
  - 行为型模式（策略、模板方法、观察者、迭代器、责任链等11种）
  - 完整代码示例、综合应用案例
  - 最佳实践、反模式注意事项

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 HTTP/TCP/IP 和设计模式文档为已完善
  - 更新文档统计（完善率提升到 37.5%）

---

## [1.4.0] - 2026-04-14

### 新增
- ✅ 扩充 Linux 常用命令文档 (`devops/linux/linux-command-shell.md`)
  - 文件系统与目录操作（11大类命令）
  - 文本处理与编辑（grep、sed、awk等）
  - 系统信息与状态监控
  - 进程管理、网络操作、用户权限管理
  - Shell 脚本基础（变量、条件、循环、函数）
  - 完整的实用脚本示例（备份脚本、监控脚本）
  - 最佳实践和常用命令速查表
- ✅ 添加标准化文档检查清单到 `CODE_STYLE_GUIDE.md`
  - 文档完整性检查（7大项）
  - 内容准确性检查（5大项）
  - 内容一致性检查（5大项）
  - 可读性检查（5大项）
  - 格式规范检查（5大项）
  - 示例有效性检查（5大项）
  - 专项检查（前端/后端/数据库/DevOps）
  - 文档发布前最终检查清单（12项）

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 标记 Linux 文档为已完善
  - 更新文档统计（完善率从 32.1% 提升到 33.9%）

---

## [1.3.0] - 2026-04-14

### 新增
- ✅ 新增 `CHANGELOG.md` 版本变更日志
- ✅ 全面完善 Docker 基础入门文档 (`devops/docker/docker-basic.md`)
  - Docker 核心概念、安装配置
  - 常用命令详解（镜像、容器、Dockerfile、Docker Compose）
  - 容器生命周期管理
  - 镜像构建与仓库使用
  - 数据卷、网络、常见问题解决方案
- ✅ 系统完善 Git 基础入门文档 (`devops/git/git-command-workflow.md`)
  - Git 安装配置、核心工作流程
  - 分支管理策略（Git Flow、GitHub Flow）
  - Conventional Commits 提交规范
  - 远程仓库操作、冲突解决方法
  - 常用命令速查表

### 优化
- ✅ 更新 `INDEX.md` 文档索引
  - 添加 `PROJECT_REVIEW_REPORT.md` 和 `CHANGELOG.md` 链接
  - 添加文档统计表格
  - 更新所有文档路径

---

## [1.2.0] - 2026-04-14

### 新增
- ✅ 新增 `PROJECT_REVIEW_REPORT.md` 项目全面复查报告
  - 项目概览与文档统计
  - 发现的问题分析
  - 改进建议（分优先级）
  - 后续工作建议
- ✅ 新增 `CODE_STYLE_GUIDE.md` 代码风格和命名规范指南
  - 通用规范、Java/JS/TS/Python/Shell 代码风格
  - Nginx/YAML/SQL 配置规范
  - Git 提交规范、检查清单
- ✅ 新增 `README.md` 项目介绍和使用指南
  - 项目概述、文档结构、已完善文档列表
  - 使用指南、贡献指南、版本历史
- ✅ 完善 Redis 数据类型文档 (`database/redis/redis-data-type.md`)
  - 5大核心数据类型完整说明
  - 丰富的可运行示例
  - 最佳实践
- ✅ 完善 Redis 常用命令文档 (`database/redis/redis-command.md`)
  - 键操作、服务器管理、发布订阅、事务、脚本、持久化
  - 完整的命令语法和使用示例
- ✅ 完善 Redis 缓存与集群文档 (`database/redis/redis-cache-cluster.md`)
  - 缓存应用场景和问题解决方案
  - 布隆过滤器、互斥锁、分布式锁完整实现
  - 主从复制、哨兵模式、Redis Cluster 配置
  - Spring Boot 集成完整示例
- ✅ 完善 Nginx 配置与反向代理文档 (`middleware/nginx/nginx-config-proxy.md`)
  - 基本配置、反向代理、负载均衡
  - 静态资源服务、SSL/TLS 配置
  - 重写跳转、安全配置、日志监控
  - 完整的生产环境配置示例

### 优化
- ✅ 修复 `INDEX.md` 文件路径问题
  - 修正所有不匹配的文件路径
  - 添加项目管理文档链接
  - 添加文档统计表格

---

## [1.1.0] - 2026-04-14

### 新增
- ✅ 完善 Spring Boot 数据访问文档 (`backend/springboot/springboot-data-access.md`)
- ✅ 完善 Spring Boot 安全认证文档 (`backend/springboot/springboot-security.md`)
- ✅ 完善 Spring Boot 中间件与部署文档 (`backend/springboot/springboot-middleware-deploy.md`)

---

## [1.0.0] - 2026-04-14

### 新增
- ✅ 初始化全栈技术知识库项目
- ✅ 创建完整的目录结构（56个文档位置）
- ✅ 完善 Spring Boot 核心自动配置文档 (`backend/springboot/springboot-core-auto-config.md`)
- ✅ 完善 Spring Boot Web MVC 文档 (`backend/springboot/springboot-web-mvc.md`)
- ✅ 完善 MyBatis-Plus 核心基础文档 (`backend/springboot/springboot-mybatis-plus.md`)
- ✅ 完善 Vue 3 核心响应式系统文档 (`frontend/vue3/vue3-core-reactivity.md`)
- ✅ 完善 Vue 3 组件 API 文档 (`frontend/vue3/vue3-component-api.md`)
- ✅ 完善 Vue 3 模板语法与指令文档 (`frontend/vue3/vue3-template-directive.md`)
- ✅ 完善 Vue 3 路由与 Pinia 状态管理文档 (`frontend/vue3/vue3-router-pinia.md`)
- ✅ 完善 Vue 3 + Vite + TypeScript 文档 (`frontend/vue3/vue3-vite-ts.md`)
- ✅ 完善 React Hooks API 文档 (`frontend/react/react-hooks-api.md`)
- ✅ 完善 React Router 文档 (`frontend/react/react-router.md`)
- ✅ 完善 Java 面向对象文档 (`backend/java-base/java-oop.md`)
- ✅ 完善 MySQL SQL 与 CRUD 文档 (`database/mysql/mysql-sql-crud.md`)
- ✅ 完善 API 调试与文档文档 (`devops/api-debug-doc.md`)
- ✅ 完善 MinIO 文件存储文档 (`middleware/minio-file-storage.md`)

---

## 文档完善进度

| 分类 | 文档总数 | 已完善 | 模板状态 | 完善率 |
|------|----------|--------|----------|--------|
| 前端技术 | 16 | 7 | 9 | 43.8% |
| 后端技术 | 17 | 10 | 7 | 58.8% |
| 数据库 | 8 | 4 | 4 | 50.0% |
| 中间件 | 6 | 3 | 3 | 50.0% |
| DevOps | 9 | 4 | 5 | 44.4% |
| 计算机基础 | 4 | 0 | 4 | 0.0% |
| **总计** | **56** | **18** | **38** | **32.1%** |

---

## 已完善文档列表

### 前端技术 (7/16)
- ✅ `frontend/vue3/vue3-core-reactivity.md` - Vue 3 核心响应式系统
- ✅ `frontend/vue3/vue3-component-api.md` - Vue 3 组件 API
- ✅ `frontend/vue3/vue3-template-directive.md` - Vue 3 模板语法与指令
- ✅ `frontend/vue3/vue3-router-pinia.md` - Vue 3 路由与 Pinia 状态管理
- ✅ `frontend/vue3/vue3-vite-ts.md` - Vue 3 + Vite + TypeScript
- ✅ `frontend/react/react-hooks-api.md` - React Hooks API
- ✅ `frontend/react/react-router.md` - React Router

### 后端技术 (10/17)
- ✅ `backend/java-base/java-oop.md` - Java 面向对象
- ✅ `backend/springboot/springboot-core-auto-config.md` - Spring Boot 核心自动配置
- ✅ `backend/springboot/springboot-web-mvc.md` - Spring Boot Web MVC
- ✅ `backend/springboot/springboot-data-access.md` - Spring Boot 数据访问
- ✅ `backend/springboot/springboot-security.md` - Spring Boot 安全认证
- ✅ `backend/springboot/springboot-middleware-deploy.md` - Spring Boot 中间件与部署
- ✅ `backend/springboot/springboot-mybatis-plus.md` - MyBatis-Plus 核心基础

### 数据库 (4/8)
- ✅ `database/mysql/mysql-sql-crud.md` - MySQL SQL 与 CRUD
- ✅ `database/redis/redis-data-type.md` - Redis 数据类型
- ✅ `database/redis/redis-command.md` - Redis 常用命令
- ✅ `database/redis/redis-cache-cluster.md` - Redis 缓存与集群

### 中间件 (3/6)
- ✅ `middleware/nginx/nginx-config-proxy.md` - Nginx 配置与反向代理
- ✅ `middleware/minio-file-storage.md` - MinIO 文件存储

### DevOps (4/9)
- ✅ `devops/api-debug-doc.md` - API 调试与文档
- ✅ `devops/docker/docker-basic.md` - Docker 基础入门
- ✅ `devops/git/git-command-workflow.md` - Git 基础入门

### 项目管理文档 (4/4)
- ✅ `README.md` - 项目介绍和使用指南
- ✅ `INDEX.md` - 文档总索引
- ✅ `CODE_STYLE_GUIDE.md` - 代码风格和命名规范
- ✅ `PROJECT_REVIEW_REPORT.md` - 项目全面复查报告
- ✅ `CHANGELOG.md` - 本文档：版本变更日志

---

## 后续计划

### 高优先级
- [ ] 扩充 Linux 常用命令文档
- [ ] 深化 HTTP/TCP/IP 核心文档
- [ ] 完善设计模式核心文档
- [ ] 添加文档检查清单

### 中优先级
- [ ] 完善 MySQL 索引与事务文档
- [ ] 完善 MySQL 性能优化文档
- [ ] 完善 RabbitMQ 核心基础文档
- [ ] 完善 Kafka 核心基础文档
- [ ] 完善 Elasticsearch 核心基础文档
- [ ] 完善 Docker Compose 文档
- [ ] 完善 Kubernetes 基础入门文档
- [ ] 完善 Git 高级功能文档

### 低优先级
- [ ] 完善前端基础文档（HTML/CSS、JavaScript、TypeScript、Axios）
- [ ] 完善 Java 基础文档（基础语法、集合、IO、并发）
- [ ] 完善 Python/Go 后端文档
- [ ] 完善计算机基础文档（网络、算法、设计模式、操作系统）
- [ ] 完善小程序文档
- [ ] 添加文档搜索功能
- [ ] 添加多语言支持

---

*最后更新：2026-04-15*