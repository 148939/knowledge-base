# 全栈技术知识库

一个完整的全栈技术知识库，内容 100% 来自官方文档，适合智能体检索调用。

## 项目概述

本文档库收录了全栈开发相关的技术知识点，涵盖前端、后端、数据库、中间件、DevOps、计算机基础等技术领域。所有文档都遵循统一的结构和代码风格，内容来自官方文档，无幻觉，适合智能体检索和开发者参考。

## 文档结构

```
knowledge-base/
├── frontend/           # 前端技术
│   ├── base/          # 前端基础（HTML/CSS、JavaScript、TypeScript、Axios）
│   ├── vue3/          # Vue 3
│   ├── react/         # React
│   ├── miniprogram/   # 小程序
│   └── uniapp/        # UniApp
├── backend/            # 后端技术
│   ├── java-base/     # Java 基础
│   ├── springboot/    # Spring Boot
│   ├── python/        # Python
│   ├── fastapi/       # FastAPI
│   ├── go/            # Go
│   └── gin/           # Gin
├── database/           # 数据库
│   ├── mysql/         # MySQL
│   ├── redis/         # Redis
│   ├── mongodb/       # MongoDB
│   └── postgresql/    # PostgreSQL
├── middleware/         # 中间件
│   ├── nginx/         # Nginx
│   ├── rabbitmq/      # RabbitMQ
│   ├── kafka/         # Kafka
│   ├── elasticsearch/ # Elasticsearch
│   └── minio/         # MinIO
├── devops/             # DevOps
│   ├── git/           # Git
│   ├── linux/         # Linux
│   ├── docker/        # Docker
│   ├── kubernetes/    # Kubernetes
│   ├── build-tools/   # 构建工具
│   └── api-debug-doc/ # API 调试与文档
└── basic/              # 计算机基础
    ├── network/       # 计算机网络
    ├── algorithm/     # 数据结构与算法
    ├── design-pattern/ # 设计模式
    └── os/            # 操作系统
```

## 文档规范

所有文档都遵循统一的结构：

```markdown
# 检索索引：本文档收录【技术领域】【主题】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

## 2. 核心知识点（结构化层级）

## 3. 官方标准语法 / API

## 4. 官方示例代码（可运行）

## 5. 官方注意事项 / 最佳实践

## 6. 官方文档参考链接
```

详细的代码风格和命名规范请参考 [CODE\_STYLE\_GUIDE.md](./CODE_STYLE_GUIDE.md)。

## 已完善文档

### 前端

- ✅ Vue 3 核心响应式系统
- ✅ Vue 3 组件 API
- ✅ Vue 3 模板语法与指令
- ✅ Vue 3 路由与 Pinia 状态管理
- ✅ Vue 3 + Vite + TypeScript
- ✅ React Hooks API
- ✅ React Router

### 后端

- ✅ Java 面向对象
- ✅ Spring Boot 核心自动配置
- ✅ Spring Boot Web MVC
- ✅ Spring Boot 数据访问
- ✅ Spring Boot 安全认证
- ✅ Spring Boot 中间件与部署
- ✅ MyBatis-Plus 核心基础

### 数据库

- ✅ MySQL SQL 与 CRUD
- ✅ Redis 数据类型
- ✅ Redis 常用命令
- ✅ Redis 缓存与集群

### 中间件

- ✅ Nginx 配置与反向代理
- ✅ MinIO 文件存储

### DevOps

- ✅ API 调试与文档

## 使用指南

### 智能体检索

本文档库专为智能体检索设计，具有以下特点：

- 统一的文档结构
- 清晰的检索索引
- 结构化的知识点层级
- 完整的代码示例
- 官方文档链接

### 开发者参考

开发者可以按技术领域查找相关文档：

1. **前端开发**：查看 `frontend/` 目录
2. **后端开发**：查看 `backend/` 目录
3. **数据库**：查看 `database/` 目录
4. **中间件**：查看 `middleware/` 目录
5. **DevOps**：查看 `devops/` 目录
6. **计算机基础**：查看 `basic/` 目录

## 贡献指南

欢迎贡献文档！在贡献前请阅读 [CODE\_STYLE\_GUIDE.md](./CODE_STYLE_GUIDE.md) 了解文档编写规范。

### 文档编写步骤

1. 选择要完善的模板文档
2. 按照统一的文档结构编写
3. 确保内容来自官方文档
4. 添加完整的代码示例
5. 添加最佳实践和官方链接
6. 遵循代码风格和命名规范

### 提交规范

提交信息请遵循 Conventional Commits 规范：

```
feat(docs): add Spring Boot Cache documentation

- Add cache configuration
- Add cache annotations
- Add best practices
```

## 版本历史

### v1.1.0 (2026-04-14)

- ✅ 完善 Redis 数据类型、常用命令、缓存与集群文档
- ✅ 完善 Nginx 配置与反向代理文档
- ✅ 创建代码风格和命名规范文档
- ✅ 创建项目 README 文档

### v1.0.0 (2026-04-14)

- ✅ 初始化知识库项目
- ✅ 创建完整的目录结构
- ✅ 完善 Spring Boot 核心文档
- ✅ 完善 Vue 3 核心文档
- ✅ 完善部分前端、后端、数据库文档

## 官方文档参考

本文档库的内容主要来自以下官方文档：

- [Spring 官方文档](https://spring.io/projects)
- [Vue 3 官方文档](https://cn.vuejs.org/)
- [React 官方文档](https://react.dev/)
- [Redis 官方文档](https://redis.io/docs/)
- [Nginx 官方文档](https://nginx.org/en/docs/)
- [MySQL 官方文档](https://dev.mysql.com/doc/)
- [Java 官方文档](https://docs.oracle.com/en/java/)
- [Python 官方文档](https://docs.python.org/zh-cn/3/)
- [Go 官方文档](https://go.dev/doc/)

## 许可证

本项目仅供学习参考使用。

## 联系方式

如有问题或建议，欢迎通过 Issue 反馈。

***

**注意**：本文档库内容均来自官方文档，如有侵权请联系删除。
