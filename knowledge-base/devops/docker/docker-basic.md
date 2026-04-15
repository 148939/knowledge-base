# 检索索引：本文档收录【Docker】【基础入门】知识点，内容100%来自Docker官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Docker 是一个开源的容器化平台，用于开发、交付和运行应用程序。Docker 使您能够将应用程序与基础设施分离，以便快速交付软件。使用 Docker，您可以像管理应用程序一样管理基础设施。通过利用 Docker 的快速交付、测试和部署代码的方法，您可以显著减少编写代码和在生产环境中运行代码之间的延迟。

Docker 的核心特性：
1. **容器化**：将应用及其依赖打包到轻量级容器中
2. **可移植性**：一次构建，到处运行
3. **隔离性**：容器之间相互隔离
4. **轻量级**：容器启动快速，资源占用少
5. **版本控制**：支持镜像版本管理
6. **易于扩展**：支持容器编排和集群管理

## 2. 核心知识点（结构化层级）

### 2.1 Docker 核心概念
1. 镜像（Image）
2. 容器（Container）
3. 仓库（Registry）
4. Dockerfile
5. Docker Compose
6. 镜像层（Layer）

### 2.2 Docker 安装配置
1. Docker 安装（Windows、macOS、Linux）
2. Docker 配置
3. Docker 镜像加速器配置
4. Docker 版本检查
5. Docker 守护进程管理

### 2.3 Docker 常用命令
1. 镜像管理命令（pull、push、build、rmi、images 等）
2. 容器管理命令（run、start、stop、restart、rm、ps 等）
3. 容器操作命令（exec、logs、cp、diff 等）
4. 系统管理命令（info、version、system df、stats 等）

### 2.4 容器生命周期管理
1. 创建容器
2. 启动容器
3. 停止容器
4. 暂停/恢复容器
5. 删除容器
6. 容器重启策略

### 2.5 镜像构建与 Dockerfile
1. Dockerfile 基础语法
2. 常用指令（FROM、RUN、COPY、ADD、CMD、ENTRYPOINT、ENV、EXPOSE、WORKDIR 等）
3. 多阶段构建
4. 镜像优化最佳实践
5. .dockerignore 文件

### 2.6 容器数据管理
1. 数据卷（Volume）
2. 绑定挂载（Bind Mount）
3. tmpfs 挂载
4. 数据卷容器
5. 数据备份与恢复

### 2.7 容器网络
1. 网络模式（bridge、host、none、container）
2. 端口映射
3. 容器间通信
4. 自定义网络
5. DNS 配置

### 2.8 镜像仓库使用
1. Docker Hub 使用
2. 私有仓库搭建
3. 镜像推送与拉取
4. 镜像标签管理

### 2.9 Docker Compose
1. Docker Compose 安装
2. docker-compose.yml 语法
3. 常用命令
4. 服务编排
5. 多服务部署

### 2.10 常见问题与解决方案
1. 容器启动失败
2. 镜像拉取失败
3. 端口冲突
4. 数据丢失问题
5. 性能优化建议

## 3. 官方标准语法 / API

### Docker 镜像管理命令
```bash
# 查看本地镜像
docker images
docker image ls

# 拉取镜像
docker pull ubuntu:latest
docker pull ubuntu:20.04
docker pull nginx:alpine

# 推送镜像
docker push username/image:tag

# 构建镜像
docker build -t myapp:v1 .
docker build -t myapp:v1 -f Dockerfile.prod .
docker build --no-cache -t myapp:v1 .

# 查看镜像历史
docker history myapp:v1

# 查看镜像详细信息
docker inspect myapp:v1

# 删除镜像
docker rmi myapp:v1
docker image rm myapp:v1
docker rmi -f myapp:v1  # 强制删除

# 删除悬空镜像
docker image prune
docker image prune -a  # 删除所有未使用的镜像

# 给镜像打标签
docker tag myapp:v1 username/myapp:v1
docker tag myapp:v1 username/myapp:latest

# 保存镜像到文件
docker save -o myapp.tar myapp:v1

# 从文件加载镜像
docker load -i myapp.tar

# 搜索镜像
docker search ubuntu
docker search --filter stars=100 ubuntu
```

### Docker 容器管理命令
```bash
# 运行容器
docker run nginx
docker run -d nginx  # 后台运行
docker run -d -p 80:80 nginx  # 端口映射
docker run -d -p 8080:80 --name mynginx nginx  # 指定名称
docker run -d -p 8080:80 -v /data:/usr/share/nginx/html nginx  # 数据卷挂载

# 交互式运行
docker run -it ubuntu /bin/bash
docker run -it --rm ubuntu /bin/bash  # 退出后自动删除

# 查看运行中的容器
docker ps
docker container ls

# 查看所有容器（包括停止的）
docker ps -a
docker container ls -a

# 启动容器
docker start mynginx
docker container start mynginx

# 停止容器
docker stop mynginx
docker container stop mynginx

# 重启容器
docker restart mynginx
docker container restart mynginx

# 强制停止容器
docker kill mynginx
docker container kill mynginx

# 暂停容器
docker pause mynginx
docker unpause mynginx

# 删除容器
docker rm mynginx
docker container rm mynginx
docker rm -f mynginx  # 强制删除运行中的容器
docker rm -v mynginx  # 删除容器同时删除数据卷

# 批量删除停止的容器
docker container prune

# 进入容器
docker exec -it mynginx /bin/bash
docker exec mynginx ls -l

# 查看容器日志
docker logs mynginx
docker logs -f mynginx  # 实时查看
docker logs --tail 100 mynginx  # 查看最后100行
docker logs -t mynginx  # 显示时间戳

# 查看容器详细信息
docker inspect mynginx

# 查看容器资源使用
docker stats
docker stats mynginx
docker stats --no-stream

# 查看容器进程
docker top mynginx

# 查看容器修改
docker diff mynginx

# 复制文件到容器
docker cp local.txt mynginx:/path/

# 从容器复制文件
docker cp mynginx:/path/file.txt ./

# 端口映射查看
docker port mynginx

# 重命名容器
docker rename oldname newname

# 更新容器
docker update --restart=always mynginx
docker update --memory 512m mynginx

# 容器提交为镜像
docker commit mynginx myimage:v1
```

### Dockerfile 指令
```dockerfile
# 基础镜像
FROM ubuntu:20.04
FROM node:18-alpine

# 设置作者（可选）
LABEL maintainer="name@example.com"

# 设置环境变量
ENV NODE_ENV=production
ENV PORT=3000

# 设置工作目录
WORKDIR /app

# 复制文件
COPY package*.json ./
COPY . .

# 添加文件（可以下载远程文件或解压）
ADD https://example.com/file.txt ./
ADD archive.tar.gz /app/

# 运行命令
RUN apt-get update && apt-get install -y curl
RUN npm install
RUN npm run build

# 暴露端口
EXPOSE 3000
EXPOSE 80/tcp

# 健康检查
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:3000/health || exit 1

# 用户切换
USER node

# 卷声明
VOLUME /app/data

# 启动命令
CMD ["npm", "start"]

# 入口点
ENTRYPOINT ["node", "server.js"]

# 多阶段构建示例
# 构建阶段
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# 生产阶段
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Docker Compose 配置
```yaml
version: '3.8'

services:
  web:
    build: .
    image: myapp:latest
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DB_HOST=db
    volumes:
      - ./data:/app/data
      - logs:/app/logs
    depends_on:
      - db
    restart: always
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3
    networks:
      - app-network

  db:
    image: mysql:8.0
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: myapp
      MYSQL_USER: user
      MYSQL_PASSWORD: password
    volumes:
      - mysql-data:/var/lib/mysql
    networks:
      - app-network

volumes:
  logs:
  mysql-data:

networks:
  app-network:
    driver: bridge
```

### Docker Compose 命令
```bash
# 启动服务
docker-compose up
docker-compose up -d  # 后台启动
docker-compose up --build  # 重新构建镜像

# 停止服务
docker-compose down
docker-compose down -v  # 同时删除数据卷

# 查看服务状态
docker-compose ps

# 查看服务日志
docker-compose logs
docker-compose logs -f
docker-compose logs web

# 执行服务命令
docker-compose exec web /bin/bash
docker-compose run web npm run test

# 重启服务
docker-compose restart
docker-compose restart web

# 停止服务
docker-compose stop
docker-compose stop web

# 启动服务
docker-compose start
docker-compose start web

# 删除服务
docker-compose rm
docker-compose rm -f

# 构建镜像
docker-compose build
docker-compose build --no-cache

# 拉取镜像
docker-compose pull

# 推送镜像
docker-compose push

# 查看配置
docker-compose config
```

## 4. 官方示例代码（可运行）

### 快速启动 Nginx 容器
```bash
# 拉取 Nginx 镜像
docker pull nginx:alpine

# 启动 Nginx 容器
docker run -d \
  --name mynginx \
  -p 8080:80 \
  -v $(pwd)/html:/usr/share/nginx/html \
  nginx:alpine

# 查看容器状态
docker ps

# 访问测试
curl http://localhost:8080

# 查看日志
docker logs mynginx

# 进入容器
docker exec -it mynginx /bin/sh

# 停止容器
docker stop mynginx

# 启动容器
docker start mynginx

# 删除容器
docker rm -f mynginx
```

### 构建 Node.js 应用镜像
```dockerfile
# Dockerfile
FROM node:18-alpine

# 设置工作目录
WORKDIR /app

# 复制 package 文件
COPY package*.json ./

# 安装依赖
RUN npm ci --only=production

# 复制应用代码
COPY . .

# 暴露端口
EXPOSE 3000

# 健康检查
HEALTHCHECK --interval=30s --timeout=3s \
  CMD curl -f http://localhost:3000/health || exit 1

# 启动应用
CMD ["npm", "start"]
```

```javascript
// server.js
const express = require('express');
const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req, res) => {
  res.json({ message: 'Hello Docker!', time: new Date() });
});

app.get('/health', (req, res) => {
  res.status(200).send('OK');
});

app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});
```

```json
// package.json
{
  "name": "docker-demo",
  "version": "1.0.0",
  "dependencies": {
    "express": "^4.18.0"
  },
  "scripts": {
    "start": "node server.js"
  }
}
```

```bash
# 构建镜像
docker build -t mynodeapp:v1 .

# 运行容器
docker run -d \
  --name mynodeapp \
  -p 3000:3000 \
  mynodeapp:v1

# 测试访问
curl http://localhost:3000

# 查看日志
docker logs -f mynodeapp
```

### 使用 Docker Compose 部署多服务应用
```yaml
# docker-compose.yml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - REDIS_HOST=redis
      - REDIS_PORT=6379
    depends_on:
      - redis
    restart: always

  redis:
    image: redis:7-alpine
    volumes:
      - redis-data:/data
    restart: always

volumes:
  redis-data:
```

```javascript
// server.js
const express = require('express');
const redis = require('redis');
const app = express();

const client = redis.createClient({
  host: process.env.REDIS_HOST,
  port: process.env.REDIS_PORT
});

client.connect();

app.get('/', async (req, res) => {
  const visits = await client.incr('visits');
  res.json({ visits: visits, time: new Date() });
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

```bash
# 启动所有服务
docker-compose up -d

# 查看服务状态
docker-compose ps

# 查看日志
docker-compose logs -f

# 测试访问
curl http://localhost:3000

# 停止服务
docker-compose down
```

### 数据卷使用示例
```bash
# 创建数据卷
docker volume create mydata

# 查看数据卷
docker volume ls
docker volume inspect mydata

# 使用数据卷启动容器
docker run -d \
  --name mysql \
  -v mydata:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  mysql:8.0

# 绑定挂载
docker run -d \
  --name nginx \
  -v $(pwd)/html:/usr/share/nginx/html \
  -p 8080:80 \
  nginx:alpine

# 从容器备份数据
docker run --rm \
  -v mydata:/data \
  -v $(pwd):/backup \
  ubuntu \
  tar czf /backup/data.tar.gz /data

# 恢复数据到容器
docker run --rm \
  -v mydata:/data \
  -v $(pwd):/backup \
  ubuntu \
  tar xzf /backup/data.tar.gz -C /
```

### 网络配置示例
```bash
# 创建自定义网络
docker network create mynetwork

# 查看网络
docker network ls
docker network inspect mynetwork

# 在网络中运行容器
docker run -d --name web --network mynetwork nginx:alpine
docker run -d --name db --network mynetwork mysql:8.0

# 容器之间通信（使用容器名）
docker exec -it web ping db

# 连接已运行的容器到网络
docker network connect mynetwork anothercontainer

# 从网络断开容器
docker network disconnect mynetwork anothercontainer

# 删除网络
docker network rm mynetwork
docker network prune  # 删除未使用的网络
```

## 5. 官方注意事项 / 最佳实践

### 镜像最佳实践
1. **使用官方基础镜像**：优先使用 Docker Hub 官方镜像
2. **选择最小化镜像**：使用 alpine 变体减小镜像体积
3. **多阶段构建**：编译和运行环境分离
4. **合并 RUN 指令**：减少镜像层数
5. **使用 .dockerignore**：排除不必要的文件
6. **不要安装不必要的包**：保持镜像精简
7. **指定具体版本标签**：避免使用 latest 标签
8. **设置非 root 用户**：提高安全性

### 容器最佳实践
1. **一个容器一个进程**：每个容器只运行一个主要进程
2. **容器是临时的**：容器可以随时被替换
3. **数据持久化到数据卷**：不要在容器层存储数据
4. **使用健康检查**：监控容器健康状态
5. **设置重启策略**：配置合适的重启策略
6. **限制资源使用**：限制 CPU 和内存使用
7. **日志输出到 stdout/stderr**：便于日志收集
8. **不要在容器中存储机密信息**：使用环境变量或 secrets

### 安全最佳实践
1. **定期更新基础镜像**：获取安全补丁
2. **扫描镜像漏洞**：使用 Trivy、Clair 等工具
3. **不要以 root 用户运行**：使用 USER 指令切换用户
4. **限制容器能力**：去掉不必要的 Linux capabilities
5. **使用只读文件系统**：对于不需要写入的容器
6. **网络隔离**：使用自定义网络隔离容器
7. **秘密管理**：使用 Docker secrets 或环境变量
8. **不要暴露不必要的端口**：只暴露需要的端口

### 性能最佳实践
1. **使用 .dockerignore**：加快构建速度
2. **利用构建缓存**：合理排序 Dockerfile 指令
3. **多阶段构建**：减小最终镜像体积
4. **使用多架构镜像**：支持多平台
5. **限制容器资源**：设置合理的 CPU 和内存限制
6. **使用数据卷**：提高 I/O 性能
7. **监控容器资源**：使用 docker stats 监控
8. **日志轮转**：避免日志文件过大

### 常见问题解决方案

#### 问题 1：容器启动后立即退出
**原因**：容器没有前台进程在运行  
**解决**：
```bash
# 查看容器日志
docker logs container_name

# 交互式运行排查
docker run -it image_name /bin/bash
```

#### 问题 2：无法连接到容器
**原因**：端口未正确映射或防火墙阻止  
**解决**：
```bash
# 检查端口映射
docker port container_name

# 检查容器是否监听端口
docker exec container_name netstat -tlnp

# 检查防火墙
sudo iptables -L -n
```

#### 问题 3：镜像拉取失败
**原因**：网络问题或镜像仓库访问受限  
**解决**：
```bash
# 配置镜像加速器
# /etc/docker/daemon.json
{
  "registry-mirrors": ["https://your-mirror.example.com"]
}

# 重启 Docker
sudo systemctl restart docker
```

#### 问题 4：数据丢失
**原因**：数据未使用数据卷，容器删除后数据丢失  
**解决**：
```bash
# 使用数据卷
docker run -v volume_name:/path image

# 定期备份数据
docker run --rm -v volume:/data -v $(pwd):/backup ubuntu tar czf /backup/data.tar.gz /data
```

#### 问题 5：Docker 磁盘空间不足
**原因**：镜像、容器、数据卷占用过多空间  
**解决**：
```bash
# 查看磁盘使用
docker system df

# 清理未使用的资源
docker system prune
docker system prune -a  # 更彻底的清理

# 清理未使用的镜像
docker image prune -a

# 清理未使用的容器
docker container prune

# 清理未使用的数据卷
docker volume prune
```

## 6. 官方文档参考链接

- Docker 官方文档：https://docs.docker.com/
- Docker 入门指南：https://docs.docker.com/get-started/
- Docker CLI 参考：https://docs.docker.com/engine/reference/commandline/cli/
- Dockerfile 参考：https://docs.docker.com/engine/reference/builder/
- Docker Compose 参考：https://docs.docker.com/compose/
- Docker 最佳实践：https://docs.docker.com/develop/dev-best-practices/
- Docker 安全最佳实践：https://docs.docker.com/engine/security/
- Docker Hub：https://hub.docker.com/
- Docker GitHub：https://github.com/docker
