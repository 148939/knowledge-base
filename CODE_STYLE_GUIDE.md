# 代码风格和命名规范指南

本文档定义了全栈技术知识库项目中所有代码示例的统一风格和命名规范。

## 一、通用规范

### 1.1 文档结构规范

所有知识库文档必须遵循以下统一结构：

```markdown
# 检索索引：本文档收录【技术领域】【主题】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

## 2. 核心知识点（结构化层级）

## 3. 官方标准语法 / API

## 4. 官方示例代码（可运行）

## 5. 官方注意事项 / 最佳实践

## 6. 官方文档参考链接
```

### 1.2 代码块规范

- 所有代码块必须指定语言标识
- 语言标识使用小写：`java`, `javascript`, `python`, `bash`, `nginx`, `yaml`, `json`, `sql` 等
- 代码块前添加说明性注释
- 代码示例应完整、可运行

```java
// ✅ 正确示例
@RestController
@RequestMapping("/api")
public class MyController {
    
    @GetMapping("/hello")
    public String hello() {
        return "Hello World";
    }
}
```

---

## 二、Java 代码风格规范

### 2.1 命名规范

| 元素类型 | 规范 | 示例 |
|---------|------|------|
| 类名 | PascalCase（大驼峰） | `UserController`, `ProductService` |
| 接口名 | PascalCase，通常以 I 开头 | `UserRepository`, `UserService` |
| 方法名 | camelCase（小驼峰），动词开头 | `getUserById()`, `createOrder()` |
| 变量名 | camelCase（小驼峰） | `userId`, `productList` |
| 常量名 | UPPER_SNAKE_CASE（全大写下划线分隔） | `MAX_SIZE`, `DEFAULT_TIMEOUT` |
| 包名 | 全小写，点分隔 | `com.example.project.controller` |

### 2.2 代码格式规范

- **缩进**：4 个空格，不使用 Tab
- **大括号**：左大括号不换行，右大括号独占一行
- **空行**：方法之间空 1 行，逻辑块之间适当空行
- **行长**：单行不超过 120 字符

```java
// ✅ 正确示例
public class UserService {
    
    private static final int MAX_RETRY_TIMES = 3;
    
    public User getUserById(Long userId) {
        if (userId == null) {
            throw new IllegalArgumentException("userId cannot be null");
        }
        
        User user = userRepository.findById(userId)
            .orElseThrow(() -> new NotFoundException("User not found"));
        
        return user;
    }
}
```

### 2.3 注解规范

- 注解单独一行或与类/方法同行
- 多个注解时，每个注解一行
- `@Override` 必须显式添加

```java
// ✅ 正确示例
@RestController
@RequestMapping("/api/users")
@RequiredArgsConstructor
public class UserController {
    
    private final UserService userService;
    
    @GetMapping("/{id}")
    @Override
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }
}
```

### 2.4 导入规范

- 按分组排序：java.* → javax.* → jakarta.* → 第三方库 → 本地包
- 每组之间空一行
- 避免使用 `*` 通配符，明确导入

```java
// ✅ 正确示例
import java.util.List;
import java.util.Optional;

import jakarta.validation.Valid;

import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import com.example.project.entity.User;
import com.example.project.service.UserService;
```

---

## 三、JavaScript/TypeScript 代码风格规范

### 3.1 命名规范

| 元素类型 | 规范 | 示例 |
|---------|------|------|
| 类名 | PascalCase | `UserService`, `ProductComponent` |
| 函数名 | camelCase，动词开头 | `getUser()`, `handleClick()` |
| 变量名 | camelCase | `userId`, `productList` |
| 常量名 | UPPER_SNAKE_CASE | `MAX_SIZE`, `API_BASE_URL` |
| React 组件 | PascalCase | `UserProfile`, `ProductList` |
| 文件名 | kebab-case 或 PascalCase | `user-service.ts`, `UserProfile.vue` |

### 3.2 代码格式规范

- **缩进**：2 个空格
- **分号**：必须使用分号
- **引号**：字符串优先使用单引号，模板字符串使用反引号
- **大括号**：左大括号不换行

```javascript
// ✅ 正确示例
const MAX_RETRY_TIMES = 3;

function getUserById(userId) {
    if (!userId) {
        throw new Error('userId is required');
    }
    
    return fetch(`/api/users/${userId}`)
        .then(response => response.json())
        .catch(error => console.error(error));
}
```

### 3.3 TypeScript 规范

- 优先使用类型注解
- 使用 `interface` 定义对象类型
- 使用 `type` 定义联合类型、工具类型
- 避免使用 `any`，使用 `unknown` 代替

```typescript
// ✅ 正确示例
interface User {
    id: number;
    name: string;
    email: string;
}

type UserRole = 'admin' | 'user' | 'guest';

async function getUserById(userId: number): Promise<User> {
    const response = await fetch(`/api/users/${userId}`);
    const user: User = await response.json();
    return user;
}
```

---

## 四、Python 代码风格规范

### 4.1 命名规范

| 元素类型 | 规范 | 示例 |
|---------|------|------|
| 类名 | PascalCase | `UserService`, `ProductController` |
| 函数名 | snake_case，动词开头 | `get_user()`, `create_order()` |
| 变量名 | snake_case | `user_id`, `product_list` |
| 常量名 | UPPER_SNAKE_CASE | `MAX_SIZE`, `DEFAULT_TIMEOUT` |
| 私有成员 | 单下划线前缀 | `_internal_method()`, `_private_var` |

### 4.2 代码格式规范

- **缩进**：4 个空格
- **行长**：单行不超过 120 字符
- **空行**：函数之间空 2 行，类方法之间空 1 行
- **导入顺序**：标准库 → 第三方库 → 本地包

```python
# ✅ 正确示例
import os
import sys
from typing import List, Optional

import requests
from fastapi import FastAPI

from .models import User
from .services import UserService

MAX_RETRY_TIMES = 3

app = FastAPI()


def get_user_by_id(user_id: int) -> Optional[User]:
    if not user_id:
        raise ValueError("user_id is required")
    
    user = UserService.get_user(user_id)
    return user
```

---

## 五、Shell/Bash 代码风格规范

### 5.1 命名规范

| 元素类型 | 规范 | 示例 |
|---------|------|------|
| 变量名 | UPPER_SNAKE_CASE | `MAX_SIZE`, `LOG_DIR` |
| 函数名 | snake_case | `setup_env()`, `cleanup()` |
| 常量名 | UPPER_SNAKE_CASE | `VERSION="1.0.0"` |

### 5.2 代码格式规范

- **缩进**：4 个空格
- **Shebang**：`#!/bin/bash` 或 `#!/usr/bin/env bash`
- **变量引用**：始终用双引号包裹变量：`"$VAR"`
- **函数定义**：使用 `function` 关键字

```bash
#!/usr/bin/env bash

# ✅ 正确示例
set -euo pipefail

LOG_DIR="/var/log/myapp"
MAX_RETRY_TIMES=3

function setup_environment() {
    local log_dir="$1"
    
    if [ ! -d "$log_dir" ]; then
        mkdir -p "$log_dir"
        echo "Created log directory: $log_dir"
    fi
}

function main() {
    setup_environment "$LOG_DIR"
    echo "Setup completed"
}

main "$@"
```

---

## 六、Nginx 配置规范

### 6.1 命名规范

- **upstream 块名**：语义化名称，如 `backend`, `static_servers`
- **server_name**：使用域名或通配符
- **location 块**：按路径组织

### 6.2 格式规范

- **缩进**：4 个空格
- **大括号**：左大括号不换行，右大括号独占一行
- **分号**：每条指令以分号结尾
- **注释**：使用 `#` 添加注释

```nginx
# ✅ 正确示例
upstream backend {
    least_conn;
    server 127.0.0.1:8080 max_fails=3 fail_timeout=30s;
    server 127.0.0.1:8081 max_fails=3 fail_timeout=30s;
    keepalive 32;
}

server {
    listen 80;
    server_name example.com;
    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }

    location /api/ {
        proxy_pass http://backend;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

---

## 七、YAML 配置规范

### 7.1 格式规范

- **缩进**：2 个空格
- **键名**：使用 snake_case
- **字符串**：可以不使用引号，除非包含特殊字符
- **列表**：使用 `-` 前缀

```yaml
# ✅ 正确示例
spring:
  application:
    name: my-app
  datasource:
    url: jdbc:mysql://localhost:3306/mydb
    username: root
    password: password
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true

logging:
  level:
    root: INFO
    com.example: DEBUG
```

---

## 八、SQL 代码规范

### 8.1 命名规范

| 元素类型 | 规范 | 示例 |
|---------|------|------|
| 表名 | snake_case，复数形式 | `users`, `orders`, `user_roles` |
| 列名 | snake_case | `user_id`, `user_name`, `created_at` |
| 主键 | `id` 或 `表名_id` | `id`, `user_id` |
| 外键 | `关联表_id` | `user_id`, `product_id` |
| 索引名 | `idx_列名` 或 `uk_列名` | `idx_user_name`, `uk_email` |

### 8.2 格式规范

- **关键字**：大写
- **表名列名**：小写 snake_case
- **缩进**：4 个空格
- **逗号**：放在行尾

```sql
-- ✅ 正确示例
CREATE TABLE users (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL UNIQUE,
    email VARCHAR(100) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_username (username),
    INDEX idx_email (email)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

SELECT 
    u.id,
    u.username,
    u.email,
    COUNT(o.id) AS order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.created_at >= '2024-01-01'
GROUP BY u.id, u.username, u.email
HAVING order_count > 5
ORDER BY order_count DESC
LIMIT 10;
```

---

## 九、Git 提交信息规范

### 9.1 提交信息格式

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 9.2 Type 类型

| 类型 | 说明 |
|------|------|
| feat | 新功能 |
| fix | 修复 bug |
| docs | 文档更新 |
| style | 代码格式（不影响代码运行） |
| refactor | 重构（既不是新增功能，也不是修复 bug） |
| perf | 性能优化 |
| test | 测试相关 |
| chore | 构建过程或辅助工具的变动 |

### 9.3 示例

```
feat(springboot): add Spring Boot Security JWT authentication

- Add JWT token provider
- Add authentication filter
- Add login endpoint
- Add refresh token support

Closes #123
```

---

## 十、其他规范

### 10.1 注释规范

- **文件头注释**：说明文件用途、作者、创建时间
- **函数注释**：说明函数功能、参数、返回值
- **行内注释**：解释复杂逻辑，不要解释显而易见的代码

```java
/**
 * 用户服务类
 * 提供用户相关的业务逻辑
 * 
 * @author Team
 * @since 1.0.0
 */
@Service
public class UserService {
    
    /**
     * 根据用户ID获取用户信息
     * 
     * @param userId 用户ID，不能为null
     * @return 用户信息
     * @throws NotFoundException 用户不存在时抛出
     */
    public User getUserById(Long userId) {
        // 省略实现...
    }
}
```

### 10.2 文件命名规范

| 文件类型 | 规范 | 示例 |
|---------|------|------|
| Java/Kotlin | PascalCase | `UserController.java` |
| JavaScript/TypeScript | camelCase 或 PascalCase | `userService.ts`, `UserProfile.tsx` |
| Vue/React 组件 | PascalCase | `UserProfile.vue`, `ProductList.tsx` |
| Python | snake_case | `user_service.py` |
| Markdown | kebab-case | `code-style-guide.md` |
| 配置文件 | kebab-case | `application-dev.yml` |

---

## 十一、文档检查清单

在完善或审核技术文档前，请对照以下检查清单进行验证。

### 11.1 文档完整性检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 检索索引 | 文档开头包含统一的检索索引格式 | ☐ |
| 核心概述 | 包含核心概述章节，有官方定义 | ☐ |
| 核心知识点 | 包含核心知识点章节，结构化为层级列表 | ☐ |
| 标准语法/API | 包含官方标准语法/API章节 | ☐ |
| 示例代码 | 包含官方示例代码章节，代码可运行 | ☐ |
| 注意事项/最佳实践 | 包含注意事项/最佳实践章节 | ☐ |
| 官方链接 | 包含官方文档参考链接章节 | ☐ |

### 11.2 内容准确性检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 官方来源 | 内容100%来自官方文档，无幻觉内容 | ☐ |
| 技术准确 | 技术描述准确无误，符合当前版本 | ☐ |
| 代码正确 | 代码示例语法正确，可直接运行 | ☐ |
| 参数说明 | 命令/API参数说明完整准确 | ☐ |
| 示例实用 | 示例代码具有实际参考价值 | ☐ |

### 11.3 内容一致性检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 文档结构 | 严格遵循6章节统一结构 | ☐ |
| 术语统一 | 技术术语在整个文档中保持一致 | ☐ |
| 命名规范 | 代码示例符合命名规范（见本规范） | ☐ |
| 格式统一 | 代码块格式、表格格式统一 | ☐ |
| 链接格式 | 官方链接格式统一，可访问 | ☐ |

### 11.4 可读性检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 层次清晰 | 标题层级清晰，逻辑结构合理 | ☐ |
| 段落分明 | 段落之间有适当空行，易于阅读 | ☐ |
| 列表规范 | 列表格式规范，标点符号正确 | ☐ |
| 代码注释 | 代码示例包含必要的注释说明 | ☐ |
| 表格清晰 | 表格列对齐，内容简洁明了 | ☐ |

### 11.5 格式规范检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 代码语言标识 | 所有代码块都有正确的语言标识 | ☐ |
| 缩进统一 | 代码缩进统一（Java 4空格，JS/TS 2空格） | ☐ |
| 引号使用 | 字符串引号使用符合语言规范 | ☐ |
| 分号使用 | 语句结束分号使用符合规范 | ☐ |
| 文件名规范 | 文档文件名使用 kebab-case | ☐ |

### 11.6 示例有效性检查

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 示例完整 | 示例代码完整，可独立运行 | ☐ |
| 依赖说明 | 需要依赖的示例有依赖说明 | ☐ |
| 配置说明 | 需要配置的示例有配置说明 | ☐ |
| 运行说明 | 有运行步骤和预期结果说明 | ☐ |
| 错误处理 | 包含常见错误和解决方案 | ☐ |

### 11.7 专项检查 - 前端文档

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| Vue/React 规范 | 组件代码符合框架规范 | ☐ |
| TypeScript 类型 | 类型定义完整准确 | ☐ |
| 响应式处理 | 响应式数据处理正确 | ☐ |
| 组件通信 | 组件通信方式说明清晰 | ☐ |

### 11.8 专项检查 - 后端文档

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| Java 代码规范 | 符合 Google Java Style Guide | ☐ |
| 注解使用 | Spring 注解使用正确 | ☐ |
| 异常处理 | 异常处理方式合理 | ☐ |
| 依赖注入 | 依赖注入方式说明清晰 | ☐ |

### 11.9 专项检查 - 数据库文档

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| SQL 语法 | SQL 语句语法正确 | ☐ |
| 命名规范 | 表名、列名命名规范 | ☐ |
| 索引说明 | 索引使用场景和注意事项 | ☐ |
| 事务说明 | 事务使用和隔离级别说明 | ☐ |

### 11.10 专项检查 - DevOps 文档

| 检查项 | 检查内容 | 状态 |
|---------|----------|------|
| 命令语法 | 命令语法和参数正确 | ☐ |
| 配置示例 | 配置文件示例完整 | ☐ |
| 安全提示 | 安全注意事项清晰 | ☐ |
| 故障处理 | 常见故障和解决方案 | ☐ |

### 11.11 文档发布前最终检查清单

在文档发布前，请确认：

- [ ] 文档完整性检查全部通过
- [ ] 内容准确性检查全部通过
- [ ] 内容一致性检查全部通过
- [ ] 可读性检查全部通过
- [ ] 格式规范检查全部通过
- [ ] 示例有效性检查全部通过
- [ ] 专项检查（根据文档类型）全部通过
- [ ] 文档已通读一遍，无明显错误
- [ ] 所有链接可正常访问
- [ ] 代码示例已验证可运行
- [ ] 已添加到 INDEX.md 文档索引
- [ ] 已更新 CHANGELOG.md 变更日志

---

## 十二、代码检查清单

在提交代码或文档前，请确认：

- [ ] 代码风格符合本规范
- [ ] 命名规范统一
- [ ] 代码格式化正确
- [ ] 必要的注释已添加
- [ ] 文档结构完整
- [ ] 代码示例可运行
- [ ] 引用的官方链接正确

---

## 十三、参考资料

- [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html)
- [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript)
- [PEP 8 - Style Guide for Python Code](https://peps.python.org/pep-0008/)
- [Conventional Commits](https://www.conventionalcommits.org/)
