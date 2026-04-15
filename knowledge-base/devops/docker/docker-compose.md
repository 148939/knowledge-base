# Docker Compose

## 1. 核心概述（官方定义）

Docker Compose 是一个用于定义和运行多容器 Docker 应用的工具。通过 Compose，可以使用 YAML 文件来配置应用的服务，然后使用一个命令就可以从配置中创建并启动所有服务。

Compose 适用于所有环境：开发、测试、CI 以及生产工作流。

---

## 2. 核心知识点（结构化层级）

### 2.1 Compose 文件结构

#### Compose 文件版本
- 版本 2.x 和 3.x
- 推荐使用最新的兼容版本

#### 顶级字段
- version：版本
- services：服务定义
- networks：网络配置
- volumes：卷配置
- configs：配置
- secrets：密钥

### 2.2 服务配置

#### 基本配置
- image：镜像
- build：构建配置
- container_name：容器名称
- command：启动命令
- entrypoint：入口点

#### 端口配置
- ports：端口映射
- expose：暴露端口

#### 卷配置
- volumes：卷挂载
- volumes_from：从其他容器挂载

#### 网络配置
- networks：网络
- network_mode：网络模式
- links：链接（已废弃）

#### 环境配置
- environment：环境变量
- env_file：环境文件

#### 依赖与重启
- depends_on：依赖关系
- restart：重启策略

### 2.3 网络配置

#### 网络类型
- bridge：默认桥接网络
- host：主机网络
- overlay：覆盖网络

#### 网络配置
- driver：网络驱动
- driver_opts：驱动选项
- ipam：IP 地址管理

### 2.4 卷配置

#### 卷类型
- named volume：命名卷
- bind mount：绑定挂载
- tmpfs：临时文件系统

#### 卷配置
- driver：卷驱动
- driver_opts：驱动选项
- external：外部卷

### 2.5 Compose 命令

#### 基本命令
- up：启动服务
- down：停止并删除
- ps：列出服务
- logs：查看日志
- exec：执行命令

#### 管理命令
- start：启动服务
- stop：停止服务
- restart：重启服务
- pause：暂停服务
- unpause：恢复服务

#### 构建命令
- build：构建服务
- pull：拉取镜像
- push：推送镜像

### 2.6 多环境配置

#### 覆盖文件
- docker-compose.yml：基础配置
- docker-compose.override.yml：覆盖配置

#### 指定文件
- -f：指定配置文件
- 多个文件合并

### 2.7 健康检查

#### 健康检查配置
- healthcheck：健康检查
- test：检查命令
- interval：间隔
- timeout：超时
- retries：重试次数

### 2.8 部署配置（Swarm）

#### 部署配置
- deploy：部署配置
- replicas：副本数
- resources：资源限制
- restart_policy：重启策略
- placement：放置约束

---

## 3. 官方标准语法 / API

### 3.1 Compose 文件语法

```yaml
# 基本结构
version: '3.8'

services:
  webapp:
    image: nginx:alpine
    container_name: webapp
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
      - logs:/var/log/nginx
    networks:
      - frontend
      - backend
    depends_on:
      - db
    environment:
      - DEBUG=true
      - DB_HOST=db
    restart: always

  db:
    image: mysql:8.0
    container_name: db
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: app
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - backend

volumes:
  logs:
  mysql-data:

networks:
  frontend:
  backend:
```

### 3.2 Compose 命令语法

```bash
# 启动服务
docker-compose up
docker-compose up -d  # 后台启动
docker-compose up --build  # 构建并启动

# 停止服务
docker-compose down
docker-compose down -v  # 删除卷

# 查看服务
docker-compose ps
docker-compose ps -a  # 包括停止的

# 查看日志
docker-compose logs
docker-compose logs -f  # 跟随输出
docker-compose logs webapp  # 指定服务

# 执行命令
docker-compose exec webapp bash
docker-compose exec db mysql -u root -p

# 构建服务
docker-compose build
docker-compose build --no-cache

# 拉取镜像
docker-compose pull

# 其他命令
docker-compose start
docker-compose stop
docker-compose restart
```

---

## 4. 官方示例代码（可运行）

### 4.1 Web 应用示例

```yaml
version: '3.8'

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./html:/usr/share/nginx/html
    depends_on:
      - app
    networks:
      - frontend

  app:
    build:
      context: ./app
      dockerfile: Dockerfile
    environment:
      - NODE_ENV=production
      - DB_HOST=db
      - DB_PORT=5432
    volumes:
      - ./app:/app
      - /app/node_modules
    depends_on:
      - db
    networks:
      - frontend
      - backend

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD: secret
      POSTGRES_DB: app
    volumes:
      - postgres-data:/var/lib/postgresql/data
      - ./init.sql:/docker-entrypoint-initdb.d/init.sql
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app"]
      interval: 10s
      timeout: 5s
      retries: 5

volumes:
  postgres-data:

networks:
  frontend:
    driver: bridge
  backend:
    driver: bridge
```

### 4.2 开发环境示例

```yaml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3000:3000"
      - "9229:9229"  # 调试端口
    volumes:
      - .:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DEBUG=true
    command: npm run dev

  db:
    image: mysql:8.0
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: dev_db
    volumes:
      - mysql-data:/var/lib/mysql

  cache:
    image: redis:7-alpine
    ports:
      - "6379:6379"
    volumes:
      - redis-data:/data

volumes:
  mysql-data:
  redis-data:
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 Compose 文件最佳实践

1. **版本选择**
   - 使用最新兼容版本
   - 考虑生产环境兼容性

2. **服务命名**
   - 使用有意义的服务名
   - 保持命名一致性

3. **配置管理**
   - 使用环境变量
   - 敏感信息使用 secrets
   - 多环境使用覆盖文件

### 5.2 开发最佳实践

1. **热重载**
   - 使用卷挂载源代码
   - 排除 node_modules 等目录

2. **调试支持**
   - 暴露调试端口
   - 配置开发工具

3. **快速迭代**
   - 使用 docker-compose up --build
   - 合理使用缓存

### 5.3 生产最佳实践

1. **安全配置**
   - 不要以 root 用户运行
   - 限制容器资源
   - 使用官方镜像

2. **持久化**
   - 使用命名卷
   - 定期备份数据

3. **健康检查**
   - 配置健康检查
   - 合理设置重启策略

---

## 6. 官方文档参考链接

- [Docker Compose 官方文档](https://docs.docker.com/compose/)
- [Compose 文件参考](https://docs.docker.com/compose/compose-file/)
- [Compose 命令行参考](https://docs.docker.com/compose/reference/)
- [Docker Compose 最佳实践](https://docs.docker.com/compose/best-practices/)
