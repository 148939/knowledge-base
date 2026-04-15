# 检索索引：本文档收录【API调试与文档】【工具使用】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

API 调试与文档是软件开发过程中的重要环节。本知识库涵盖主流的 API 调试工具（Postman、Apifox、curl）和 API 文档工具（Swagger、OpenAPI、SpringDoc）。这些工具帮助开发者更高效地设计、测试、调试和管理 API，提升开发效率和 API 质量。

## 2. 核心知识点（结构化层级）

### 2.1 Postman 使用
1. 基础功能
   - 创建请求
   - 请求方法（GET、POST、PUT、DELETE等）
   - 请求参数
   - 请求头
   - 请求体
2. 集合管理
   - 创建集合
   - 组织请求
   - 集合变量
3. 环境变量
   - 环境配置
   - 全局变量
   - 变量引用
4. 前置脚本
   - 请求前处理
   - 动态参数
5. 后置脚本
   - 响应解析
   - 断言测试
6. Mock 服务
7. 监视器
8. Newman 命令行运行

### 2.2 Apifox 使用
1. 项目管理
2. API 设计
3. API 调试
4. 自动化测试
5. Mock 服务
6. 文档生成
7. 数据导入导出
8. 团队协作

### 2.3 curl 命令
1. 基础用法
2. HTTP 方法
3. 请求头
4. 请求数据
5. 文件上传下载
6. Cookie 管理
7. 代理设置
8. SSL/TLS

### 2.4 Swagger/OpenAPI
1. OpenAPI 规范
2. Swagger UI
3. Swagger Editor
4. 注解使用
5. API 文档配置
6. 接口分组
7. 安全配置
8. 离线文档

### 2.5 SpringDoc
1. SpringDoc 集成
2. 常用注解
3. 分组配置
4. 安全配置
5. 自定义配置
6. Knife4j 增强

## 3. 官方标准语法 / API

### Postman 使用
```javascript
// 前置脚本（Pre-request Script）
// 生成时间戳
pm.globals.set("timestamp", new Date().getTime());

// 生成随机字符串
pm.globals.set("randomId", Math.random().toString(36).substring(7));

// 生成 UUID
function generateUUID() {
    return 'xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx'.replace(/[xy]/g, function(c) {
        var r = Math.random() * 16 | 0, v = c == 'x' ? r : (r & 0x3 | 0x8);
        return v.toString(16);
    });
}
pm.globals.set("uuid", generateUUID());

// 后置脚本（Tests）
// 状态码断言
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

// 响应时间断言
pm.test("Response time is less than 500ms", function () {
    pm.expect(pm.response.responseTime).to.be.below(500);
});

// 响应体断言
pm.test("Response has required fields", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property('code');
    pm.expect(jsonData).to.have.property('data');
});

// 保存响应数据到变量
var jsonData = pm.response.json();
pm.environment.set("token", jsonData.data.token);
pm.environment.set("userId", jsonData.data.id);
```

### curl 命令
```bash
# 基础 GET 请求
curl https://api.example.com/users

# POST 请求
curl -X POST https://api.example.com/users \
  -H "Content-Type: application/json" \
  -d '{"name":"John","email":"john@example.com"}'

# PUT 请求
curl -X PUT https://api.example.com/users/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"John Doe"}'

# DELETE 请求
curl -X DELETE https://api.example.com/users/1

# 带请求头
curl https://api.example.com/users \
  -H "Authorization: Bearer token123" \
  -H "Accept: application/json"

# 保存响应到文件
curl https://api.example.com/users -o output.json

# 显示响应头
curl -i https://api.example.com/users

# 只显示响应头
curl -I https://api.example.com/users

# 跟随重定向
curl -L https://api.example.com/users

# 文件上传
curl -X POST https://api.example.com/upload \
  -F "file=@/path/to/file.txt"
```

### SpringDoc 配置
```xml
<!-- pom.xml -->
<dependency>
    <groupId>org.springdoc</groupId>
    <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
    <version>2.3.0</version>
</dependency>

<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
    <version>4.4.0</version>
</dependency>
```

```yaml
# application.yml
springdoc:
  api-docs:
    enabled: true
    path: /v3/api-docs
  swagger-ui:
    enabled: true
    path: /swagger-ui.html
```

```java
@Configuration
public class SpringDocConfig {
    
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("API 文档")
                        .version("1.0.0")
                        .description("项目 API 接口文档"))
                .addSecurityItem(new SecurityRequirement().addList("Bearer Authentication"))
                .components(new Components()
                        .addSecuritySchemes("Bearer Authentication",
                                new SecurityScheme()
                                        .type(SecurityScheme.Type.HTTP)
                                        .scheme("bearer")
                                        .bearerFormat("JWT")));
    }
}

@RestController
@RequestMapping("/users")
@Tag(name = "用户管理", description = "用户相关接口")
public class UserController {
    
    @Operation(summary = "获取用户列表", description = "分页获取用户列表")
    @GetMapping
    public Result<PageResult<UserDTO>> list(
            @RequestParam(defaultValue = "1") Integer page,
            @RequestParam(defaultValue = "10") Integer size) {
        return Result.success();
    }
    
    @Operation(summary = "获取用户详情", description = "根据ID获取用户详情")
    @GetMapping("/{id}")
    public Result<UserDTO> getById(@PathVariable Long id) {
        return Result.success();
    }
    
    @Operation(summary = "创建用户", description = "创建新用户")
    @PostMapping
    public Result<UserDTO> create(@RequestBody CreateUserDTO userDTO) {
        return Result.success();
    }
}
```

## 4. 官方示例代码（可运行）

### Postman 测试脚本示例
```javascript
// 完整接口测试
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Response has code field", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("code");
    pm.expect(jsonData.code).to.eql(200);
});

pm.test("Response has data field", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData).to.have.property("data");
});

pm.test("Response time is acceptable", function () {
    pm.expect(pm.response.responseTime).to.be.below(1000);
});
```

### curl 常用命令
```bash
#!/bin/bash
BASE_URL="https://api.example.com"

# 登录并获取 token
LOGIN_RESPONSE=$(curl -s -X POST "$BASE_URL/auth/login" \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"admin123"}')

echo "登录响应: $LOGIN_RESPONSE"

# 获取用户列表
curl -s -X GET "$BASE_URL/users?page=1&size=10" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Accept: application/json"
```

## 5. 官方注意事项 / 最佳实践

### Postman 使用注意事项
1. **环境管理**：合理使用环境变量，不同环境配置不同变量
2. **集合组织**：按业务模块组织集合，便于管理和分享
3. **变量命名**：变量命名清晰规范，避免冲突
4. **脚本安全**：前置/后置脚本注意安全，不要泄露敏感信息
5. **Mock 服务**：后端未完成时使用 Mock 服务开发前端
6. **自动化测试**：利用 Newman 在 CI/CD 中自动化运行 API 测试

### Swagger/SpringDoc 最佳实践
1. **注解完整**：为所有 API 添加完整的注解说明
2. **示例数据**：提供合理的示例数据，便于测试
3. **分组管理**：按业务模块分组，便于查阅
4. **安全配置**：合理配置安全认证信息
5. **文档维护**：代码变更时同步更新文档

### curl 使用注意事项
1. **参数转义**：注意特殊字符的转义
2. **错误处理**：检查 curl 命令的返回值
3. **安全传输**：生产环境使用 HTTPS
4. **超时设置**：合理设置超时时间

### 最佳实践总结
1. **API 优先设计**：先设计 API 文档再开发代码
2. **接口版本管理**：合理管理 API 版本
3. **统一响应格式**：统一 API 响应格式
4. **错误码规范**：制定统一的错误码规范
5. **自动化测试**：利用工具自动化 API 测试
6. **文档同步**：保持代码和文档同步更新
7. **权限控制**：合理控制 API 访问权限
8. **性能监控**：监控 API 性能和可用性
9. **日志审计**：记录 API 访问日志便于审计
10. **团队协作**：利用工具促进团队协作

## 6. 官方文档参考链接

- Postman 官方文档：https://learning.postman.com/
- Apifox 官方文档：https://apifox.com/help/
- curl 官方文档：https://curl.se/docs/
- Swagger 官方文档：https://swagger.io/docs/
- OpenAPI 规范：https://spec.openapis.org/oas/v3.1.0
- SpringDoc 官方文档：https://springdoc.org/
- Knife4j 官方文档：https://doc.xiaominfo.com/
