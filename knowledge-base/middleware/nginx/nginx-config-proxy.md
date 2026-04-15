# 检索索引：本文档收录【Nginx】【配置与反向代理】知识点，内容100%来自Nginx官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Nginx 是一个高性能的 HTTP 和反向代理服务器，也是一个 IMAP/POP3/SMTP 代理服务器。Nginx 以其高性能、高稳定性、丰富的功能集、简单的配置和低资源消耗而闻名。

Nginx 的核心特性：
1. **高性能**：事件驱动架构，支持高并发连接
2. **反向代理**：支持 HTTP、HTTPS、TCP、UDP 代理
3. **负载均衡**：支持多种负载均衡算法
4. **静态资源服务**：高性能的静态文件服务
5. **缓存**：支持响应缓存和代理缓存
6. **SSL/TLS 支持**：支持 HTTPS 和 HTTP/2
7. **灵活的配置**：简单易懂的配置语法

## 2. 核心知识点（结构化层级）

### 2.1 基本配置结构
1. 主配置文件 nginx.conf
2. 配置块（main、events、http、server、location）
3. 指令和变量
4. include 指令
5. 配置文件组织

### 2.2 核心模块
1. ngx_http_core_module - HTTP 核心模块
2. ngx_http_proxy_module - 代理模块
3. ngx_http_upstream_module - 负载均衡模块
4. ngx_http_fastcgi_module - FastCGI 模块
5. ngx_http_ssl_module - SSL 模块
6. ngx_http_gzip_module - 压缩模块
7. ngx_http_rewrite_module - 重写模块

### 2.3 反向代理配置
1. proxy_pass 指令
2. proxy_set_header 设置请求头
3. proxy_redirect 重定向
4. proxy_buffers 缓冲区设置
5. proxy_cache 缓存配置
6. proxy_connect_timeout 连接超时
7. proxy_send_timeout 发送超时
8. proxy_read_timeout 读取超时

### 2.4 负载均衡配置
1. upstream 块
2. server 指令
3. weight 权重
4. max_fails 最大失败次数
5. fail_timeout 失败超时
6. backup 备份服务器
7. down 标记为不可用
8. 负载均衡算法（轮询、加权轮询、IP hash、least_conn 等）

### 2.5 静态资源服务
1. root 和 alias 指令
2. index 指令
3. autoindex 目录列表
4. expires 缓存过期
5. try_files 尝试文件

### 2.6 SSL/TLS 配置
1. listen 443 ssl
2. ssl_certificate 证书
3. ssl_certificate_key 私钥
4. ssl_protocols 协议版本
5. ssl_ciphers 加密套件
6. ssl_prefer_server_ciphers
7. HTTP/2 支持
8. HSTS 配置

### 2.7 重写和跳转
1. return 指令
2. rewrite 指令
3. if 判断
4. break 和 last
5. 正则表达式
6. 变量使用

### 2.8 安全配置
1. 限制访问（allow、deny）
2. auth_basic 基本认证
3. limit_req 限制请求速率
4. limit_conn 限制连接数
5. 隐藏版本号
6. 安全响应头

### 2.9 日志和监控
1. access_log 访问日志
2. error_log 错误日志
3. log_format 日志格式
4. open_log_file_cache 日志文件缓存
5. status 模块
6. stub_status 状态监控

## 3. 官方标准语法 / API

### 基本配置结构
```nginx
# 主配置文件 /etc/nginx/nginx.conf

# 全局块 - main
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

# 事件块 - events
events {
    worker_connections 1024;
    use epoll;
    multi_accept on;
}

# HTTP 块 - http
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    tcp_nopush     on;
    tcp_nodelay    on;
    keepalive_timeout  65;
    types_hash_max_size 2048;
    client_max_body_size 10m;

    # Gzip 压缩
    gzip  on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css text/xml text/javascript
               application/javascript application/json application/xml
               application/rss+xml font/truetype font/opentype
               application/vnd.ms-fontobject image/svg+xml;

    # 包含其他配置文件
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;
}
```

### 反向代理配置
```nginx
# 简单的反向代理
server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        # 超时设置
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        
        # 缓冲区设置
        proxy_buffering on;
        proxy_buffer_size 4k;
        proxy_buffers 8 4k;
        proxy_busy_buffers_size 8k;
    }
}

# 路径反向代理
server {
    listen 80;
    server_name example.com;

    # /api 路径代理到后端服务
    location /api/ {
        proxy_pass http://127.0.0.1:8080/;  # 注意末尾的斜杠
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    # /static 路径代理到静态资源服务
    location /static/ {
        proxy_pass http://127.0.0.1:9000/;
        expires 30d;
        add_header Cache-Control "public, immutable";
    }
}

# WebSocket 反向代理
server {
    listen 80;
    server_name example.com;

    location /ws/ {
        proxy_pass http://127.0.0.1:8080;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        
        # WebSocket 超时设置
        proxy_read_timeout 86400;
    }
}

# 代理缓存配置
proxy_cache_path /var/cache/nginx levels=1:2 keys_zone=my_cache:10m max_size=1g inactive=60m use_temp_path=off;

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_cache my_cache;
        proxy_cache_valid 200 302 10m;
        proxy_cache_valid 404 1m;
        proxy_cache_valid any 5m;
        proxy_cache_key "$scheme$request_method$host$request_uri";
        proxy_cache_use_stale error timeout invalid_header updating http_500 http_502 http_503 http_504;
        add_header X-Proxy-Cache $upstream_cache_status;
    }
}
```

### 负载均衡配置
```nginx
# 简单的轮询负载均衡
upstream backend {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}

# 加权轮询
upstream backend {
    server 127.0.0.1:8080 weight=3;
    server 127.0.0.1:8081 weight=2;
    server 127.0.0.1:8082 weight=1;
}

# IP Hash（客户端 IP 固定访问同一后端）
upstream backend {
    ip_hash;
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

# 最少连接数
upstream backend {
    least_conn;
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

# 最短响应时间（需要 nginx-plus）
upstream backend {
    least_time header;
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    server 127.0.0.1:8082;
}

# 健康检查和参数配置
upstream backend {
    server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
    server 127.0.0.1:8081 max_fails=3 fail_timeout=30s;
    server 127.0.0.1:8082 backup;  # 备份服务器
    server 127.0.0.1:8083 down;    # 标记为不可用
    keepalive 32;  # 空闲连接数
}

# keepalive 配置
upstream backend {
    server 127.0.0.1:8080;
    server 127.0.0.1:8081;
    keepalive 32;
}

server {
    listen 80;
    server_name example.com;

    location / {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

### 静态资源服务配置
```nginx
# 基本静态资源服务
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}

# root 和 alias 的区别
server {
    listen 80;
    server_name example.com;

    # root: /static/image.jpg -> /var/www/static/image.jpg
    location /static/ {
        root /var/www;
    }

    # alias: /static/image.jpg -> /var/www/assets/image.jpg
    location /static/ {
        alias /var/www/assets/;  # 注意末尾的斜杠
    }
}

# 带缓存的静态资源服务
server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    # 图片、字体等静态资源长期缓存
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
        access_log off;
    }

    # HTML 文件不缓存
    location ~* \.html$ {
        expires -1;
        add_header Cache-Control "no-cache, no-store, must-revalidate";
    }
}

# 目录列表
server {
    listen 80;
    server_name example.com;
    root /var/www/downloads;

    location / {
        autoindex on;
        autoindex_exact_size off;
        autoindex_localtime on;
    }
}

# SPA 单页应用路由支持
server {
    listen 80;
    server_name example.com;
    root /var/www/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
    }
}
```

### SSL/TLS 配置
```nginx
# 基本 HTTPS 配置
server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers off;

    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

# HTTP 重定向到 HTTPS
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;

    return 301 https://$server_name$request_uri;
}

# 使用 Let's Encrypt 证书
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    root /var/www/html;
    location / {
        try_files $uri $uri/ =404;
    }
}

# HSTS 配置
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/cert.pem;
    ssl_certificate_key /etc/nginx/ssl/key.pem;

    # HSTS - 告诉浏览器只使用 HTTPS 访问
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;

    root /var/www/html;
    location / {
        try_files $uri $uri/ =404;
    }
}

# 安全的 SSL 配置（现代）
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/nginx/ssl/fullchain.pem;
    ssl_certificate_key /etc/nginx/ssl/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;

    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;

    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;

    add_header Strict-Transport-Security "max-age=63072000" always;
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";

    root /var/www/html;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 重写和跳转配置
```nginx
# 简单的 301 永久重定向
server {
    listen 80;
    server_name old.example.com;
    return 301 https://new.example.com$request_uri;
}

# www 重定向到非 www
server {
    listen 80;
    server_name www.example.com;
    return 301 $scheme://example.com$request_uri;
}

# 非 www 重定向到 www
server {
    listen 80;
    server_name example.com;
    return 301 $scheme://www.example.com$request_uri;
}

# rewrite 重写
server {
    listen 80;
    server_name example.com;
    root /var/www/html;

    # 重写 /old/xxx 到 /new/xxx
    rewrite ^/old/(.*)$ /new/$1 permanent;

    # 重写 /product/123 到 /product.php?id=123
    rewrite ^/product/(\d+)$ /product.php?id=$1 last;

    location / {
        try_files $uri $uri/ =404;
    }
}

# if 判断（谨慎使用）
server {
    listen 80;
    server_name example.com;

    # 如果是移动端，重定向到移动端网站
    if ($http_user_agent ~* "(android|bb\d+|meego).+mobile|avantgo|bada\/|blackberry|blazer|compal|elaine|fennec|hiptop|iemobile|ip(hone|od)|iris|kindle|lge |maemo|midp|mmp|mobile.+firefox|netfront|opera m(ob|in)i|palm( os)?|phone|p(ixi|re)\/|plucker|pocket|psp|series(4|6)0|symbian|treo|up\.(browser|link)|vodafone|wap|windows ce|xda|xiino" ) {
        return 301 https://m.example.com$request_uri;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}
```

### 安全配置
```nginx
# 限制 IP 访问
server {
    listen 80;
    server_name example.com;
    root /var/www/html;

    location /admin/ {
        allow 192.168.1.0/24;
        allow 10.0.0.0/8;
        deny all;
        
        # 或反向
        # deny 192.168.1.100;
        # allow all;
    }
}

# 基本认证
server {
    listen 80;
    server_name example.com;
    root /var/www/html;

    location /protected/ {
        auth_basic "Restricted Access";
        auth_basic_user_file /etc/nginx/.htpasswd;
    }
}

# 使用 htpasswd 创建密码文件
# htpasswd -c /etc/nginx/.htpasswd username
# htpasswd /etc/nginx/.htpasswd anotheruser

# 限制请求速率
limit_req_zone $binary_remote_addr zone=req_limit:10m rate=10r/s;

server {
    listen 80;
    server_name example.com;

    limit_req zone=req_limit burst=20 nodelay;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

# 限制连接数
limit_conn_zone $binary_remote_addr zone=conn_limit:10m;

server {
    listen 80;
    server_name example.com;

    limit_conn conn_limit 10;
    
    location / {
        try_files $uri $uri/ =404;
    }
}

# 隐藏 Nginx 版本号
http {
    server_tokens off;
    
    # 其他配置...
}

# 安全响应头
server {
    listen 80;
    server_name example.com;

    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;
    add_header Content-Security-Policy "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval'; style-src 'self' 'unsafe-inline';" always;

    location / {
        try_files $uri $uri/ =404;
    }
}

# 防止 SQL 注入（简单示例）
server {
    listen 80;
    server_name example.com;

    location / {
        if ($query_string ~* "union.*select.*\(") {
            return 403;
        }
        if ($query_string ~* "concat.*\(") {
            return 403;
        }
        
        try_files $uri $uri/ =404;
    }
}
```

### 日志和监控配置
```nginx
# 自定义日志格式
http {
    log_format main '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent" '
                    '$request_time $upstream_response_time';

    log_format json escape=json '{'
        '"time_local":"$time_local",'
        '"remote_addr":"$remote_addr",'
        '"remote_user":"$remote_user",'
        '"request":"$request",'
        '"status":"$status",'
        '"body_bytes_sent":"$body_bytes_sent",'
        '"http_referer":"$http_referer",'
        '"http_user_agent":"$http_user_agent",'
        '"request_time":"$request_time",'
        '"upstream_response_time":"$upstream_response_time"'
    '}';

    # 访问日志
    access_log /var/log/nginx/access.log main;
    
    # 错误日志
    error_log /var/log/nginx/error.log warn;
}

# 按条件记录日志
server {
    listen 80;
    server_name example.com;

    # 静态资源不记录日志
    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 30d;
        access_log off;
    }

    # 不同 location 使用不同日志格式
    location /api/ {
        proxy_pass http://127.0.0.1:8080;
        access_log /var/log/nginx/api.log main;
    }

    location / {
        try_files $uri $uri/ =404;
    }
}

# stub_status 状态监控
server {
    listen 80;
    server_name localhost;

    location /nginx_status {
        stub_status on;
        access_log off;
        allow 127.0.0.1;
        allow 192.168.1.0/24;
        deny all;
    }
}

# stub_status 输出示例:
# Active connections: 291
# server accepts handled requests
#  16630948 16630948 31070465
# Reading: 6 Writing: 179 Waiting: 106
```

## 4. 官方示例代码（可运行）

### 完整的生产环境配置示例
```nginx
# nginx.conf - 主配置文件
user nginx;
worker_processes auto;
error_log /var/log/nginx/error.log warn;
pid /var/run/nginx.pid;

events {
    worker_connections 10240;
    use epoll;
    multi_accept on;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for" '
                      '$request_time $upstream_response_time';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    tcp_nopush     on;
    tcp_nodelay    on;
    keepalive_timeout  65;
    types_hash_max_size 2048;
    client_max_body_size 50m;
    client_body_buffer_size 128k;

    # Gzip 压缩
    gzip  on;
    gzip_vary on;
    gzip_min_length 1024;
    gzip_comp_level 6;
    gzip_proxied expired no-cache no-store private auth;
    gzip_types text/plain text/css text/xml text/javascript
               application/javascript application/json application/xml
               application/rss+xml font/truetype font/opentype
               application/vnd.ms-fontobject image/svg+xml;

    # 隐藏版本号
    server_tokens off;

    # 限流配置
    limit_req_zone $binary_remote_addr zone=req_limit:10m rate=10r/s;
    limit_conn_zone $binary_remote_addr zone=conn_limit:10m;

    # 上游服务
    upstream backend {
        least_conn;
        server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
        server 127.0.0.1:8081 max_fails=3 fail_timeout=30s;
        keepalive 32;
    }

    upstream static {
        server 127.0.0.1:9000;
    }

    # 包含站点配置
    include /etc/nginx/conf.d/*.conf;
}

# /etc/nginx/conf.d/example.com.conf - 站点配置
server {
    listen 80;
    listen [::]:80;
    server_name example.com www.example.com;

    # HTTP 重定向到 HTTPS
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    server_name example.com www.example.com;

    # SSL 证书
    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    # SSL 配置
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers off;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;

    # 安全响应头
    add_header Strict-Transport-Security "max-age=63072000; includeSubDomains" always;
    add_header X-Frame-Options "DENY" always;
    add_header X-Content-Type-Options "nosniff" always;
    add_header X-XSS-Protection "1; mode=block" always;
    add_header Referrer-Policy "strict-origin-when-cross-origin" always;

    # 限流
    limit_req zone=req_limit burst=20 nodelay;
    limit_conn conn_limit 50;

    # 网站根目录
    root /var/www/html;
    index index.html;

    # www 重定向到非 www
    if ($host = www.example.com) {
        return 301 https://example.com$request_uri;
    }

    # 静态资源
    location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff|woff2|ttf|eot)$ {
        expires 1y;
        add_header Cache-Control "public, immutable";
        access_log off;
        try_files $uri =404;
    }

    # API 代理
    location /api/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Connection "";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_connect_timeout 60s;
        proxy_send_timeout 60s;
        proxy_read_timeout 60s;
        proxy_buffering on;
    }

    # WebSocket
    location /ws/ {
        proxy_pass http://backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_read_timeout 86400;
    }

    # SPA 单页应用路由
    location / {
        try_files $uri $uri/ /index.html;
    }

    # 禁止访问隐藏文件
    location ~ /\. {
        deny all;
        access_log off;
        log_not_found off;
    }
}

# /etc/nginx/conf.d/status.conf - 状态监控
server {
    listen 127.0.0.1:8080;
    server_name localhost;

    location /nginx_status {
        stub_status on;
        access_log off;
        allow 127.0.0.1;
        deny all;
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 基本配置最佳实践
1. **合理设置 worker_processes**：通常设置为 auto 或 CPU 核心数
2. **优化 worker_connections**：根据并发量设置，通常 10240 足够
3. **启用 sendfile**：提高文件传输效率
4. **配置 keepalive_timeout**：合理设置连接超时时间
5. **限制 client_max_body_size**：防止大文件上传攻击

### 反向代理最佳实践
1. **设置正确的请求头**：Host、X-Real-IP、X-Forwarded-For 等
2. **配置超时时间**：proxy_connect_timeout、proxy_send_timeout、proxy_read_timeout
3. **使用 keepalive 连接**：减少连接建立开销
4. **启用 proxy_buffering**：提高性能
5. **配置代理缓存**：缓存静态内容和不常变化的内容

### 负载均衡最佳实践
1. **选择合适的算法**：轮询、加权轮询、IP hash、least_conn
2. **配置健康检查**：max_fails 和 fail_timeout
3. **使用备份服务器**：backup 标记备用服务器
4. **配置 keepalive**：复用连接提高性能
5. **监控后端状态**：及时发现故障节点

### 静态资源服务最佳实践
1. **使用 expires 缓存**：设置合理的缓存时间
2. **使用 Cache-Control**：精细控制缓存策略
3. **启用 Gzip 压缩**：减少传输大小
4. **区分 root 和 alias**：理解两者的区别
5. **配置 SPA 路由**：使用 try_files 支持前端路由

### SSL/TLS 最佳实践
1. **使用现代协议**：TLSv1.2 和 TLSv1.3
2. **配置安全的加密套件**：使用强加密算法
3. **启用 HSTS**：强制使用 HTTPS
4. **配置 OCSP Stapling**：提高证书验证速度
5. **定期更新证书**：使用 Let's Encrypt 自动续期

### 安全最佳实践
1. **隐藏版本号**：server_tokens off
2. **配置安全响应头**：X-Frame-Options、X-XSS-Protection 等
3. **限制访问**：使用 allow/deny 控制 IP 访问
4. **启用基本认证**：保护敏感路径
5. **配置限流**：limit_req 和 limit_conn 防止 DDoS
6. **禁止访问隐藏文件**：location ~ /\. { deny all; }

### 性能优化最佳实践
1. **启用 Gzip 压缩**：减少传输数据量
2. **配置缓存**：静态资源长期缓存
3. **使用 sendfile**：提高文件传输效率
4. **优化缓冲区**：合理配置 proxy_buffers
5. **启用连接复用**：keepalive 减少连接开销
6. **使用 HTTP/2**：支持多路复用

### 日志和监控最佳实践
1. **配置访问日志**：记录关键信息
2. **自定义日志格式**：包含需要的字段
3. **配置错误日志**：warn 级别足够
4. **使用 stub_status**：监控 Nginx 状态
5. **日志轮转**：使用 logrotate 管理日志文件
6. **监控关键指标**：连接数、请求数、响应时间等

## 6. 官方文档参考链接

- Nginx 官方文档：https://nginx.org/en/docs/
- Nginx 初学者指南：https://nginx.org/en/docs/beginners_guide.html
- ngx_http_core_module：https://nginx.org/en/docs/http/ngx_http_core_module.html
- ngx_http_proxy_module：https://nginx.org/en/docs/http/ngx_http_proxy_module.html
- ngx_http_upstream_module：https://nginx.org/en/docs/http/ngx_http_upstream_module.html
- ngx_http_ssl_module：https://nginx.org/en/docs/http/ngx_http_ssl_module.html
- ngx_http_gzip_module：https://nginx.org/en/docs/http/ngx_http_gzip_module.html
- ngx_http_rewrite_module：https://nginx.org/en/docs/http/ngx_http_rewrite_module.html
- Nginx 配置最佳实践：https://www.nginx.com/blog/tuning-nginx/
- Nginx GitHub：https://github.com/nginx/nginx
