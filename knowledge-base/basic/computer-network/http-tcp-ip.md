# 检索索引：本文档收录【计算机网络】【HTTP/TCP/IP核心】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

### TCP/IP 协议族
TCP/IP（Transmission Control Protocol/Internet Protocol，传输控制协议/网际协议）是指能够在多个不同网络间实现信息传输的协议簇。TCP/IP 协议不仅仅指的是 TCP 和 IP 两个协议，而是指一个由 FTP、SMTP、TCP、UDP、IP 等协议构成的协议簇，只是因为 TCP 协议和 IP 协议在该协议簇中最具代表性，所以被称为 TCP/IP 协议族。

### HTTP 协议
HTTP（HyperText Transfer Protocol，超文本传输协议）是一个简单的请求-响应协议，它通常运行在 TCP 之上。它指定了客户端可能发送给服务器什么样的消息以及得到什么样的响应。HTTP 是万维网（World Wide Web）的基础，也是现代互联网应用最常用的协议之一。

### HTTPS 协议
HTTPS（HyperText Transfer Protocol Secure，超文本传输安全协议）是 HTTP 协议的安全版本，通过在 HTTP 下加入 SSL/TLS 层来对数据进行加密传输，提供数据完整性和身份验证。

## 2. 核心知识点（结构化层级）

### 2.1 TCP/IP 协议栈
1. 四层模型（应用层、传输层、网络层、网络接口层）
2. OSI 七层模型对比
3. 各层主要协议
4. 数据封装与解封装
5. 协议栈工作原理

### 2.2 网络层 - IP 协议
1. IP 地址（IPv4、IPv6）
2. IP 地址分类（A、B、C、D、E类）
3. 子网划分与子网掩码
4. CIDR 无类别域间路由
5. IP 数据包格式
6. ICMP 协议（Internet Control Message Protocol）
7. ARP 协议（Address Resolution Protocol）

### 2.3 传输层 - TCP 协议
1. TCP 特点（可靠、面向连接、字节流）
2. TCP 三次握手
3. TCP 四次挥手
4. TCP 数据包格式
5. TCP 序列号与确认号
6. TCP 流量控制（滑动窗口）
7. TCP 拥塞控制
8. TCP 状态转换

### 2.4 传输层 - UDP 协议
1. UDP 特点（不可靠、无连接、数据报）
2. UDP 数据包格式
3. UDP 与 TCP 对比
4. UDP 适用场景
5. UDP 常见应用

### 2.5 HTTP 协议基础
1. HTTP 协议特点
2. HTTP 请求/响应模型
3. HTTP 版本（HTTP/1.0、HTTP/1.1、HTTP/2、HTTP/3）
4. HTTP 消息结构
5. URI/URL/URN
6. HTTP 方法（GET、POST、PUT、DELETE、PATCH等）
7. HTTP 状态码（1xx、2xx、3xx、4xx、5xx）
8. HTTP 请求头
9. HTTP 响应头
10. HTTP 实体头

### 2.6 HTTP 请求详解
1. 请求行格式
2. 请求方法详解
   - GET：获取资源
   - POST：提交数据
   - PUT：更新资源
   - DELETE：删除资源
   - PATCH：部分更新
   - HEAD：获取头部
   - OPTIONS：获取允许的方法
   - CONNECT：建立隧道
   - TRACE：追踪请求
3. 请求头字段
   - Host：目标主机
   - User-Agent：客户端信息
   - Accept：可接受的内容类型
   - Accept-Language：可接受的语言
   - Accept-Encoding：可接受的编码
   - Authorization：认证信息
   - Cookie：Cookie信息
   - Content-Type：请求体类型
   - Content-Length：请求体长度
   - Referer：来源页面
   - Origin：跨域源
   - Cache-Control：缓存控制
   - If-Modified-Since：缓存验证
   - If-None-Match：缓存验证

### 2.7 HTTP 响应详解
1. 状态行格式
2. 状态码详解
   - 1xx：信息性状态码
   - 2xx：成功状态码
   - 3xx：重定向状态码
   - 4xx：客户端错误状态码
   - 5xx：服务器错误状态码
3. 响应头字段
   - Server：服务器信息
   - Content-Type：响应体类型
   - Content-Length：响应体长度
   - Content-Encoding：内容编码
   - Content-Language：内容语言
   - Last-Modified：最后修改时间
   - ETag：实体标签
   - Cache-Control：缓存控制
   - Expires：过期时间
   - Set-Cookie：设置Cookie
   - Location：重定向地址
   - Access-Control-Allow-Origin：CORS 允许源
   - Access-Control-Allow-Methods：CORS 允许方法
   - Access-Control-Allow-Headers：CORS 允许头

### 2.8 HTTP 缓存机制
1. 缓存的作用与优势
2. 强缓存（Cache-Control、Expires）
3. 协商缓存（Last-Modified/If-Modified-Since、ETag/If-None-Match）
4. 缓存决策流程图
5. 缓存最佳实践
6. 缓存策略示例

### 2.9 Cookie 与 Session
1. Cookie 概述
2. Cookie 属性（Name、Value、Domain、Path、Expires/Max-Age、Secure、HttpOnly、SameSite）
3. Cookie 安全
4. Session 概述
5. Session 工作原理
6. Session 存储方式
7. Cookie vs Session 对比
8. Token 认证机制

### 2.10 HTTPS 加密原理
1. HTTPS 概述
2. SSL/TLS 协议
3. 对称加密与非对称加密
4. 数字证书与 CA
5. HTTPS 握手过程
6. HTTPS 性能优化
7. HTTPS 配置示例
8. HTTPS 安全最佳实践

### 2.11 HTTP/2 与 HTTP/3
1. HTTP/2 新特性
   - 二进制分帧
   - 多路复用
   - 服务器推送
   - 头部压缩
2. HTTP/2 配置
3. HTTP/3 概述
4. QUIC 协议
5. HTTP/3 优势

### 2.12 跨域与 CORS
1. 同源策略
2. 跨域场景
3. CORS（Cross-Origin Resource Sharing）
4. CORS 请求类型（简单请求、预检请求）
5. CORS 响应头
6. 跨域解决方案汇总
7. CORS 配置示例

### 2.13 Web 安全基础
1. XSS（跨站脚本攻击）
2. CSRF（跨站请求伪造）
3. SQL 注入
4. 点击劫持
5. MITM（中间人攻击）
6. 安全最佳实践
7. HTTP 安全头（X-XSS-Protection、X-Frame-Options、X-Content-Type-Options、Content-Security-Policy）

### 2.14 网络诊断工具
1. ping：测试连通性
2. traceroute/mtr：路由追踪
3. telnet/nc：端口测试
4. curl：HTTP 请求
5. netstat/ss：网络连接
6. tcpdump/Wireshark：抓包分析
7. dig/nslookup：DNS 查询

## 3. 官方标准语法 / API

### HTTP 请求格式
```http
GET /index.html HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
Accept: text/html,application/xhtml+xml
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Connection: keep-alive

```

### HTTP POST 请求
```http
POST /api/users HTTP/1.1
Host: api.example.com
Content-Type: application/json
Content-Length: 55
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...

{
  "name": "Zhang San",
  "email": "zhangsan@example.com"
}
```

### HTTP 响应格式
```http
HTTP/1.1 200 OK
Date: Tue, 14 Apr 2024 12:00:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Type: application/json
Content-Length: 123
Cache-Control: public, max-age=3600

{
  "success": true,
  "data": {
    "id": 1,
    "name": "Zhang San",
    "email": "zhangsan@example.com"
  }
}
```

### 常见 HTTP 状态码
```http
# 1xx 信息性
100 Continue
101 Switching Protocols

# 2xx 成功
200 OK
201 Created
202 Accepted
204 No Content

# 3xx 重定向
301 Moved Permanently
302 Found
303 See Other
304 Not Modified
307 Temporary Redirect
308 Permanent Redirect

# 4xx 客户端错误
400 Bad Request
401 Unauthorized
403 Forbidden
404 Not Found
405 Method Not Allowed
409 Conflict
410 Gone
429 Too Many Requests

# 5xx 服务器错误
500 Internal Server Error
501 Not Implemented
502 Bad Gateway
503 Service Unavailable
504 Gateway Timeout
```

### Set-Cookie 头示例
```http
Set-Cookie: sessionId=abc123; Path=/; HttpOnly; Secure; SameSite=Lax
Set-Cookie: theme=dark; Path=/; Max-Age=31536000
Set-Cookie: userPref=language=zh-CN; Path=/
```

### Cache-Control 头示例
```http
# 不缓存
Cache-Control: no-store

# 可以缓存，但使用前必须验证
Cache-Control: no-cache

# 私有缓存（仅浏览器）
Cache-Control: private, max-age=3600

# 公共缓存（浏览器和CDN）
Cache-Control: public, max-age=3600, s-maxage=86400

# 协商缓存
Cache-Control: must-revalidate
```

### CORS 响应头示例
```http
Access-Control-Allow-Origin: https://www.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Allow-Credentials: true
Access-Control-Max-Age: 86400
```

### HTTP 安全头示例
```http
X-XSS-Protection: 1; mode=block
X-Frame-Options: DENY
X-Content-Type-Options: nosniff
Content-Security-Policy: default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'; img-src 'self' data:
Strict-Transport-Security: max-age=31536000; includeSubDomains
Referrer-Policy: strict-origin-when-cross-origin
```

### cURL 命令示例
```bash
# 简单 GET 请求
curl https://api.example.com/users

# 带请求头
curl -H "Authorization: Bearer token" https://api.example.com/users

# POST 请求
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Zhang San","email":"zhangsan@example.com"}'

# PUT 请求
curl -X PUT https://api.example.com/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Li Si"}'

# DELETE 请求
curl -X DELETE https://api.example.com/users/1

# 显示响应头
curl -i https://api.example.com/users

# 只显示响应头
curl -I https://api.example.com/users

# 跟随重定向
curl -L https://example.com

# 保存响应到文件
curl -o response.json https://api.example.com/users

# 显示详细信息
curl -v https://api.example.com/users
```

### Nginx HTTPS 配置示例
```nginx
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    
    server_name example.com www.example.com;
    
    # SSL 证书配置
    ssl_certificate /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;
    
    # SSL 配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers on;
    
    # HSTS
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    
    # 安全头
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    
    # CSP
    add_header Content-Security-Policy "default-src 'self'; script-src 'self'; style-src 'self'; img-src 'self' data:" always;
    
    root /var/www/html;
    index index.html;
    
    location / {
        try_files $uri $uri/ =404;
    }
    
    location /api/ {
        proxy_pass http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

# HTTP 重定向到 HTTPS
server {
    listen 80;
    listen [::]:80;
    
    server_name example.com www.example.com;
    
    return 301 https://$server_name$request_uri;
}
```

### Node.js Express CORS 配置
```javascript
const express = require('express');
const cors = require('cors');

const app = express();

// 简单 CORS 配置
app.use(cors());

// 自定义 CORS 配置
app.use(cors({
  origin: 'https://www.example.com',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization'],
  credentials: true,
  maxAge: 86400
}));

// 动态 origin
app.use(cors({
  origin: function (origin, callback) {
    const whitelist = ['https://www.example.com', 'https://app.example.com'];
    if (whitelist.indexOf(origin) !== -1 || !origin) {
      callback(null, true);
    } else {
      callback(new Error('Not allowed by CORS'));
    }
  }
}));

app.get('/api/users', (req, res) => {
  res.json({ users: [] });
});

app.listen(3000);
```

### Java Spring Boot CORS 配置
```java
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class CorsConfig implements WebMvcConfigurer {
    
    @Override
    public void addCorsMappings(CorsRegistry registry) {
        registry.addMapping("/**")
                .allowedOrigins("https://www.example.com")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .allowCredentials(true)
                .maxAge(3600);
    }
}

// 控制器级别配置
import org.springframework.web.bind.annotation.CrossOrigin;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@CrossOrigin(origins = "https://www.example.com")
public class UserController {
    
    @GetMapping("/api/users")
    public List<User> getUsers() {
        return userService.findAll();
    }
}
```

## 4. 官方示例代码（可运行）

### TCP 三次握手详解
```
客户端                                      服务器
  |                                           |
  | 1. SYN = 1, seq = x                      |
  | -------------------------------------->  |
  |                                           |
  | 2. SYN = 1, ACK = 1, seq = y, ack = x+1|
  | <--------------------------------------  |
  |                                           |
  | 3. ACK = 1, seq = x+1, ack = y+1        |
  | -------------------------------------->  |
  |                                           |
  |       连接建立，开始传输数据              |
```

### TCP 四次挥手详解
```
客户端                                      服务器
  |                                           |
  | 1. FIN = 1, seq = x                      |
  | -------------------------------------->  |
  |                                           |
  | 2. ACK = 1, seq = y, ack = x+1          |
  | <--------------------------------------  |
  |                                           |
  | 3. FIN = 1, ACK = 1, seq = z, ack = x+1|
  | <--------------------------------------  |
  |                                           |
  | 4. ACK = 1, seq = x+1, ack = z+1        |
  | -------------------------------------->  |
  |                                           |
  |       连接关闭                              |
```

### HTTP 缓存工作流程
```
浏览器                                      服务器
  |                                           |
  | 1. 请求资源                              |
  | -------------------------------------->  |
  |                                           |
  | 2. 响应 + Cache-Control: max-age=3600   |
  | <--------------------------------------  |
  |                                           |
  |    3. 本地缓存（1小时内）                 |
  |                                           |
  | 4. 1小时后，再次请求                     |
  |    If-Modified-Since: xxx                |
  | -------------------------------------->  |
  |                                           |
  | 5. 304 Not Modified（未修改）            |
  | <--------------------------------------  |
  |    或 200 OK + 新内容（已修改）          |
```

### HTTPS 握手过程
```
客户端                                      服务器
  |                                           |
  | 1. Client Hello                          |
  |    (随机数1, 加密套件列表)              |
  | -------------------------------------->  |
  |                                           |
  | 2. Server Hello                          |
  |    (随机数2, 选定加密套件)              |
  |    Certificate (证书)                     |
  | <--------------------------------------  |
  |                                           |
  | 3. 验证证书，生成预主密钥                |
  |    Client Key Exchange (加密的预主密钥)  |
  | -------------------------------------->  |
  |                                           |
  | 4. 双方计算会话密钥                       |
  |                                           |
  | 5. Change Cipher Spec + Finished        |
  | <-------------------------------------->  |
  |                                           |
  |       开始加密传输                        |
```

### 完整的 HTTP 请求响应示例

#### 示例 1：GET 请求获取用户列表
```bash
# 请求
curl -v https://api.example.com/users

# 完整请求
GET /users HTTP/1.1
Host: api.example.com
User-Agent: curl/7.68.0
Accept: */*

# 完整响应
HTTP/1.1 200 OK
Date: Tue, 14 Apr 2024 12:00:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Type: application/json
Content-Length: 156
Cache-Control: public, max-age=300

{
  "success": true,
  "data": [
    { "id": 1, "name": "Zhang San", "email": "zhangsan@example.com" },
    { "id": 2, "name": "Li Si", "email": "lisi@example.com" }
  ]
}
```

#### 示例 2：POST 请求创建用户
```bash
# 请求
curl -v -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Wang Wu","email":"wangwu@example.com"}'

# 完整请求
POST /users HTTP/1.1
Host: api.example.com
User-Agent: curl/7.68.0
Content-Type: application/json
Content-Length: 61

{"name":"Wang Wu","email":"wangwu@example.com"}

# 完整响应
HTTP/1.1 201 Created
Date: Tue, 14 Apr 2024 12:00:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Type: application/json
Content-Length: 120
Location: https://api.example.com/users/3

{
  "success": true,
  "data": {
    "id": 3,
    "name": "Wang Wu",
    "email": "wangwu@example.com"
  }
}
```

#### 示例 3：304 Not Modified 缓存响应
```bash
# 第一次请求
curl -v https://api.example.com/static/image.png

# 响应
HTTP/1.1 200 OK
Date: Tue, 14 Apr 2024 12:00:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Content-Type: image/png
Content-Length: 12345
Last-Modified: Mon, 01 Apr 2024 10:00:00 GMT
ETag: "abc123xyz"
Cache-Control: public, max-age=3600

[二进制图片数据]

# 第二次请求（带缓存验证头）
curl -v \
  -H "If-Modified-Since: Mon, 01 Apr 2024 10:00:00 GMT" \
  -H "If-None-Match: \"abc123xyz\"" \
  https://api.example.com/static/image.png

# 响应（304 Not Modified，没有响应体）
HTTP/1.1 304 Not Modified
Date: Tue, 14 Apr 2024 12:05:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Last-Modified: Mon, 01 Apr 2024 10:00:00 GMT
ETag: "abc123xyz"
Cache-Control: public, max-age=3600
```

#### 示例 4：CORS 预检请求
```bash
# 预检请求（OPTIONS）
curl -v -X OPTIONS https://api.example.com/users \
  -H "Origin: https://www.example.com" \
  -H "Access-Control-Request-Method: POST" \
  -H "Access-Control-Request-Headers: Content-Type"

# 预检响应
HTTP/1.1 204 No Content
Date: Tue, 14 Apr 2024 12:00:00 GMT
Server: Apache/2.4.41 (Ubuntu)
Access-Control-Allow-Origin: https://www.example.com
Access-Control-Allow-Methods: GET, POST, PUT, DELETE, OPTIONS
Access-Control-Allow-Headers: Content-Type
Access-Control-Max-Age: 86400
Vary: Origin

# 实际请求（POST）
curl -v -X POST https://api.example.com/users \
  -H "Origin: https://www.example.com" \
  -H "Content-Type: application/json" \
  -d '{"name":"Zhang San"}'

# 实际响应
HTTP/1.1 200 OK
Date: Tue, 14 Apr 2024 12:00:01 GMT
Server: Apache/2.4.41 (Ubuntu)
Access-Control-Allow-Origin: https://www.example.com
Vary: Origin
Content-Type: application/json
Content-Length: 100

{"success": true}
```

## 5. 官方注意事项 / 最佳实践

### HTTP 协议最佳实践

#### 请求方法使用原则
1. **GET**：用于获取资源，不应有副作用
2. **POST**：用于创建资源或提交数据
3. **PUT**：用于完整更新资源（幂等）
4. **PATCH**：用于部分更新资源
5. **DELETE**：用于删除资源（幂等）
6. **HEAD**：用于获取头部信息（与GET类似但无响应体）
7. **OPTIONS**：用于获取允许的方法

#### 状态码使用原则
1. **200 OK**：请求成功
2. **201 Created**：资源创建成功
3. **204 No Content**：成功但无响应体
4. **301 Moved Permanently**：永久重定向
5. **302 Found**：临时重定向（考虑用 307/308）
6. **304 Not Modified**：资源未修改，使用缓存
7. **400 Bad Request**：客户端请求错误
8. **401 Unauthorized**：未认证
9. **403 Forbidden**：无权限
10. **404 Not Found**：资源不存在
11. **405 Method Not Allowed**：方法不允许
12. **409 Conflict**：资源冲突
13. **429 Too Many Requests**：请求过于频繁
14. **500 Internal Server Error**：服务器内部错误
15. **503 Service Unavailable**：服务不可用

#### 缓存最佳实践
1. **静态资源**：使用强缓存（Cache-Control: max-age=31536000）
2. **API 响应**：根据业务场景选择缓存策略
3. **文件指纹**：静态资源使用哈希文件名（app.abc123.js）
4. **协商缓存**：配合 Last-Modified 和 ETag 使用
5. **CDN 缓存**：合理设置 s-maxage
6. **Cache-Control 优先级**：max-age > Expires
7. **避免过度缓存**：敏感数据不应缓存
8. **缓存验证**：合理使用 must-revalidate

#### Cookie 安全最佳实践
1. **HttpOnly**：防止 XSS 窃取 Cookie
2. **Secure**：仅通过 HTTPS 传输
3. **SameSite**：防止 CSRF 攻击（Lax/Strict）
4. **Path**：限制 Cookie 作用路径
5. **Domain**：限制 Cookie 作用域
6. **Expires/Max-Age**：合理设置过期时间
7. **不要存储敏感信息**：密码、密钥等不要存 Cookie
8. **Cookie 大小限制**：单 Cookie 不超过 4KB

### HTTPS 最佳实践
1. **使用最新 TLS 版本**：TLS 1.2 或 TLS 1.3
2. **使用强加密套件**：优先使用 AEAD 加密套件
3. **配置 HSTS**：强制使用 HTTPS
4. **证书链完整**：确保证书链正确配置
5. **定期更新证书**：避免证书过期
6. **禁用旧协议**：禁用 SSL 3.0、TLS 1.0、TLS 1.1
7. **HTTP 重定向 HTTPS**：301 永久重定向
8. **使用安全头**：配置 X-Frame-Options、CSP 等
9. **OCSP Stapling**：启用 OCSP 装订
10. **Perfect Forward Secrecy**：使用支持 PFS 的加密套件

### 性能优化最佳实践
1. **使用 HTTP/2 或 HTTP/3**：多路复用提升性能
2. **资源压缩**：gzip 或 Brotli 压缩
3. **图片优化**：使用 WebP、AVIF 现代格式
4. **资源合并**：减少请求数量
5. **CDN 加速**：静态资源使用 CDN
6. **预加载**：关键资源使用 preload
7. **懒加载**：非关键资源延迟加载
8. **DNS 预解析**：dns-prefetch
9. **TCP 优化**：TCP Fast Open、BBR
10. **连接复用**：Keep-Alive

### 安全最佳实践
1. **输入验证**：所有输入都必须验证
2. **输出编码**：防止 XSS 攻击
3. **使用参数化查询**：防止 SQL 注入
4. **CSRF Token**：防止 CSRF 攻击
5. **安全头**：配置 CSP、X-Frame-Options 等
6. **HTTPS 全站**：全站启用 HTTPS
7. **密码安全**：使用 bcrypt/argon2 哈希
8. **会话管理**：安全的 Session 管理
9. **速率限制**：防止暴力破解
10. **错误处理**：不暴露详细错误信息

### 常见问题排查

#### 问题 1：跨域错误
**症状**：浏览器控制台显示 CORS 错误  
**排查**：
1. 检查服务端 CORS 配置
2. 检查 Origin 头是否在白名单
3. 检查请求方法是否允许
4. 检查请求头是否允许
5. 检查是否需要 credentials

#### 问题 2：缓存不生效
**症状**：每次都重新请求资源  
**排查**：
1. 检查 Cache-Control 头
2. 检查 Expires 头
3. 检查是否有 Pragma: no-cache
4. 检查文件指纹是否变化
5. 检查 CDN 配置

#### 问题 3：HTTPS 证书错误
**症状**：浏览器提示证书不安全  
**排查**：
1. 检查证书是否过期
2. 检查证书域名是否匹配
3. 检查证书链是否完整
4. 检查是否使用自签名证书
5. 检查系统时间是否正确

## 6. 官方文档参考链接

- HTTP/1.1 规范：https://datatracker.ietf.org/doc/html/rfc2616
- HTTP/2 规范：https://datatracker.ietf.org/doc/html/rfc7540
- HTTP/3 规范：https://datatracker.ietf.org/doc/html/rfc9114
- TLS 1.3 规范：https://datatracker.ietf.org/doc/html/rfc8446
- MDN HTTP 文档：https://developer.mozilla.org/zh-CN/docs/Web/HTTP
- MDN Web 安全：https://developer.mozilla.org/zh-CN/docs/Web/Security
- CORS 规范：https://fetch.spec.whatwg.org/#http-cors-protocol
- OWASP：https://owasp.org/
- Nginx 文档：https://nginx.org/en/docs/
- Apache 文档：https://httpd.apache.org/docs/
