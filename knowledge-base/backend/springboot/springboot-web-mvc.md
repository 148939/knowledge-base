# 检索索引：本文档收录【Spring Boot】【Web MVC】知识点，内容100%来自Spring官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Spring Boot Web MVC 是 Spring Boot 提供的 Web 开发框架，它基于 Spring MVC，提供了自动配置和快速开发能力。Spring Boot Web MVC 支持 RESTful API 开发、视图渲染、文件上传、异常处理等功能，通过 @RestController、@Controller、@RequestMapping 等注解简化 Web 应用开发。

## 2. 核心知识点（结构化层级）

### 2.1 控制器（Controller）
1. @Controller - 视图控制器
2. @RestController - REST 控制器
3. @RequestMapping - 请求映射
4. @GetMapping、@PostMapping、@PutMapping、@DeleteMapping
5. @PatchMapping

### 2.2 请求处理
1. @PathVariable - 路径变量
2. @RequestParam - 请求参数
3. @RequestBody - 请求体
4. @RequestHeader - 请求头
5. @CookieValue - Cookie 值
6. @RequestAttribute - 请求属性
7. @SessionAttribute - Session 属性

### 2.3 响应处理
1. @ResponseBody - 响应体
2. ResponseEntity - 响应实体
3. @ResponseStatus - 响应状态
4. HttpHeaders - 响应头

### 2.4 数据验证
1. @Valid - 验证注解
2. @Validated - 分组验证
3. JSR-380 验证注解：@NotNull、@NotBlank、@NotEmpty、@Min、@Max 等
4. BindingResult - 验证结果
5. MethodArgumentNotValidException 异常处理

### 2.5 异常处理
1. @ExceptionHandler - 异常处理器
2. @ControllerAdvice - 全局异常处理
3. @RestControllerAdvice - REST 全局异常处理
4. ResponseEntityExceptionHandler - 基础异常处理器
5. ErrorController - 自定义错误控制器

### 2.6 文件上传下载
1. MultipartFile - 文件上传
2. @RequestPart - 文件部件
3. MultipartHttpServletRequest
4. 下载文件
5. 文件大小限制

### 2.7 视图技术
1. Thymeleaf 模板引擎
2. JSP 支持
3. FreeMarker 支持
4. 视图解析器配置
5. 静态资源处理

### 2.8 拦截器和过滤器
1. HandlerInterceptor - 拦截器
2. WebMvcConfigurer - MVC 配置
3. Filter - 过滤器
4. OncePerRequestFilter
5. 拦截器注册

## 3. 官方标准语法 / API

### 基础控制器示例
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.ResponseEntity;
import java.util.List;
import java.util.ArrayList;

// REST 控制器
@RestController
@RequestMapping("/api/users")
public class UserController {

    private List<User> users = new ArrayList<>();

    // GET - 获取所有用户
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(users);
    }

    // GET - 根据ID获取用户
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return users.stream()
                .filter(user -> user.getId().equals(id))
                .findFirst()
                .map(ResponseEntity::ok)
                .orElse(ResponseEntity.notFound().build());
    }

    // POST - 创建用户
    @PostMapping
    public ResponseEntity<User> createUser(@RequestBody User user) {
        user.setId((long) (users.size() + 1));
        users.add(user);
        return ResponseEntity.status(201).body(user);
    }

    // PUT - 更新用户
    @PutMapping("/{id}")
    public ResponseEntity<User> updateUser(@PathVariable Long id, @RequestBody User userDetails) {
        return users.stream()
                .filter(user -> user.getId().equals(id))
                .findFirst()
                .map(user -> {
                    user.setName(userDetails.getName());
                    user.setEmail(userDetails.getEmail());
                    return ResponseEntity.ok(user);
                })
                .orElse(ResponseEntity.notFound().build());
    }

    // DELETE - 删除用户
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        boolean removed = users.removeIf(user -> user.getId().equals(id));
        return removed ? ResponseEntity.noContent().build() : ResponseEntity.notFound().build();
    }
}

// 用户实体类
class User {
    private Long id;
    private String name;
    private String email;

    // getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
}
```

### 请求参数处理
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import java.util.Map;
import java.util.List;

@RestController
@RequestMapping("/api/params")
public class RequestParamsController {

    // 路径变量
    @GetMapping("/path/{id}/name/{name}")
    public String pathVariables(@PathVariable Long id, @PathVariable String name) {
        return "ID: " + id + ", Name: " + name;
    }

    // 路径变量别名
    @GetMapping("/alias/{userId}")
    public String pathVariableAlias(@PathVariable("userId") Long id) {
        return "User ID: " + id;
    }

    // 请求参数 - 单个
    @GetMapping("/single")
    public String singleParam(@RequestParam String name) {
        return "Name: " + name;
    }

    // 请求参数 - 可选
    @GetMapping("/optional")
    public String optionalParam(@RequestParam(required = false, defaultValue = "Guest") String name) {
        return "Name: " + name;
    }

    // 请求参数 - Map
    @GetMapping("/map")
    public String mapParams(@RequestParam Map<String, String> params) {
        return "Params: " + params.toString();
    }

    // 请求参数 - List
    @GetMapping("/list")
    public String listParams(@RequestParam List<String> ids) {
        return "IDs: " + ids.toString();
    }

    // 请求头
    @GetMapping("/headers")
    public String requestHeaders(
            @RequestHeader("User-Agent") String userAgent,
            @RequestHeader(value = "X-Custom-Header", required = false) String customHeader) {
        return "User-Agent: " + userAgent + ", Custom: " + customHeader;
    }

    // Cookie
    @GetMapping("/cookies")
    public String cookies(@CookieValue(value = "JSESSIONID", required = false) String sessionId) {
        return "Session ID: " + sessionId;
    }

    // 请求体
    @PostMapping("/body")
    public String requestBody(@RequestBody Map<String, Object> body) {
        return "Body: " + body.toString();
    }

    // 文件上传
    @PostMapping("/upload")
    public String uploadFile(@RequestParam("file") MultipartFile file) {
        return "Uploaded: " + file.getOriginalFilename() + ", Size: " + file.getSize();
    }

    // 多文件上传
    @PostMapping("/upload/multiple")
    public String uploadMultipleFiles(@RequestParam("files") MultipartFile[] files) {
        StringBuilder sb = new StringBuilder();
        for (MultipartFile file : files) {
            sb.append(file.getOriginalFilename()).append(", ");
        }
        return "Uploaded: " + sb.toString();
    }
}
```

### ResponseEntity 响应处理
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.*;
import java.net.URI;
import java.util.HashMap;
import java.util.Map;

@RestController
@RequestMapping("/api/response")
public class ResponseController {

    // 简单响应
    @GetMapping("/simple")
    public ResponseEntity<String> simpleResponse() {
        return ResponseEntity.ok("Hello World");
    }

    // 自定义状态码
    @GetMapping("/status")
    public ResponseEntity<String> customStatus() {
        return ResponseEntity.status(HttpStatus.ACCEPTED).body("Accepted");
    }

    // 201 Created 响应
    @PostMapping("/created")
    public ResponseEntity<Map<String, Object>> createdResponse(@RequestBody Map<String, Object> data) {
        Map<String, Object> result = new HashMap<>();
        result.put("id", 1);
        result.putAll(data);
        
        URI location = URI.create("/api/response/1");
        return ResponseEntity.created(location).body(result);
    }

    // 204 No Content 响应
    @DeleteMapping("/no-content")
    public ResponseEntity<Void> noContentResponse() {
        return ResponseEntity.noContent().build();
    }

    // 404 Not Found 响应
    @GetMapping("/not-found")
    public ResponseEntity<String> notFoundResponse() {
        return ResponseEntity.notFound().build();
    }

    // 400 Bad Request 响应
    @GetMapping("/bad-request")
    public ResponseEntity<String> badRequestResponse() {
        return ResponseEntity.badRequest().body("Invalid request");
    }

    // 自定义响应头
    @GetMapping("/headers")
    public ResponseEntity<String> customHeaders() {
        return ResponseEntity.ok()
                .header("X-Custom-Header", "value")
                .header("X-Another-Header", "another-value")
                .body("With custom headers");
    }

    // Content-Type 响应头
    @GetMapping("/content-type")
    public ResponseEntity<String> contentTypeResponse() {
        return ResponseEntity.ok()
                .contentType(MediaType.APPLICATION_JSON)
                .body("{\"message\": \"Hello\"}");
    }

    // 缓存控制
    @GetMapping("/cache")
    public ResponseEntity<String> cacheControl() {
        return ResponseEntity.ok()
                .cacheControl(CacheControl.maxAge(30, java.util.concurrent.TimeUnit.MINUTES))
                .body("Cached for 30 minutes");
    }

    // ETag
    @GetMapping("/etag")
    public ResponseEntity<String> etagResponse() {
        return ResponseEntity.ok()
                .eTag("\"abc123\"")
                .body("With ETag");
    }

    // 最后修改时间
    @GetMapping("/last-modified")
    public ResponseEntity<String> lastModifiedResponse() {
        return ResponseEntity.ok()
                .lastModified(System.currentTimeMillis())
                .body("With Last-Modified");
    }

    // 完整的响应实体
    @GetMapping("/full")
    public ResponseEntity<Map<String, Object>> fullResponse() {
        Map<String, Object> body = new HashMap<>();
        body.put("message", "Hello");
        body.put("timestamp", System.currentTimeMillis());

        HttpHeaders headers = new HttpHeaders();
        headers.setContentType(MediaType.APPLICATION_JSON);
        headers.set("X-Custom", "value");

        return new ResponseEntity<>(body, headers, HttpStatus.OK);
    }
}
```

### 数据验证
```java
import org.springframework.validation.annotation.Validated;
import org.springframework.web.bind.annotation.*;
import org.springframework.http.ResponseEntity;
import javax.validation.Valid;
import javax.validation.constraints.*;
import java.util.List;

// 验证实体类
@Validated
class UserDTO {
    
    @NotNull(message = "ID 不能为空")
    private Long id;
    
    @NotBlank(message = "用户名不能为空")
    @Size(min = 2, max = 50, message = "用户名长度必须在 2-50 之间")
    private String username;
    
    @NotBlank(message = "邮箱不能为空")
    @Email(message = "邮箱格式不正确")
    private String email;
    
    @NotBlank(message = "密码不能为空")
    @Size(min = 6, max = 20, message = "密码长度必须在 6-20 之间")
    private String password;
    
    @Min(value = 0, message = "年龄不能小于 0")
    @Max(value = 150, message = "年龄不能大于 150")
    private Integer age;
    
    @Pattern(regexp = "^1[3-9]\\d{9}$", message = "手机号格式不正确")
    private String phone;
    
    @NotEmpty(message = "标签不能为空")
    private List<String> tags;
    
    // getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
    public String getPhone() { return phone; }
    public void setPhone(String phone) { this.phone = phone; }
    public List<String> getTags() { return tags; }
    public void setTags(List<String> tags) { this.tags = tags; }
}

@RestController
@RequestMapping("/api/validation")
@Validated
public class ValidationController {

    // @Valid 验证请求体
    @PostMapping("/user")
    public ResponseEntity<String> createUser(@Valid @RequestBody UserDTO user) {
        return ResponseEntity.ok("User created: " + user.getUsername());
    }

    // 验证路径变量
    @GetMapping("/user/{id}")
    public ResponseEntity<String> getUserById(
            @PathVariable @Min(value = 1, message = "ID 必须大于 0") Long id) {
        return ResponseEntity.ok("User ID: " + id);
    }

    // 验证请求参数
    @GetMapping("/search")
    public ResponseEntity<String> search(
            @RequestParam @NotBlank(message = "关键词不能为空") String keyword,
            @RequestParam @Min(value = 1, message = "页码必须大于 0") Integer page,
            @RequestParam @Min(value = 10, message = "每页至少 10 条") 
            @Max(value = 100, message = "每页最多 100 条") Integer size) {
        return ResponseEntity.ok("Search: " + keyword + ", Page: " + page + ", Size: " + size);
    }
}
```

### 全局异常处理
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.http.*;
import org.springframework.validation.BindException;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.servlet.mvc.method.annotation.ResponseEntityExceptionHandler;
import java.util.*;
import java.time.LocalDateTime;

// 自定义业务异常
class BusinessException extends RuntimeException {
    private String errorCode;
    
    public BusinessException(String errorCode, String message) {
        super(message);
        this.errorCode = errorCode;
    }
    
    public String getErrorCode() {
        return errorCode;
    }
}

// 资源未找到异常
class ResourceNotFoundException extends RuntimeException {
    public ResourceNotFoundException(String message) {
        super(message);
    }
}

// 错误响应类
class ErrorResponse {
    private LocalDateTime timestamp;
    private int status;
    private String error;
    private String message;
    private String path;
    private Map<String, String> errors;
    
    public ErrorResponse(int status, String error, String message, String path) {
        this.timestamp = LocalDateTime.now();
        this.status = status;
        this.error = error;
        this.message = message;
        this.path = path;
    }
    
    // getters and setters
    public LocalDateTime getTimestamp() { return timestamp; }
    public int getStatus() { return status; }
    public String getError() { return error; }
    public String getMessage() { return message; }
    public String getPath() { return path; }
    public Map<String, String> getErrors() { return errors; }
    public void setErrors(Map<String, String> errors) { this.errors = errors; }
}

// 全局异常处理器
@RestControllerAdvice
public class GlobalExceptionHandler extends ResponseEntityExceptionHandler {

    // 处理业务异常
    @ExceptionHandler(BusinessException.class)
    public ResponseEntity<ErrorResponse> handleBusinessException(
            BusinessException ex,
            HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
                HttpStatus.BAD_REQUEST.value(),
                ex.getErrorCode(),
                ex.getMessage(),
                request.getRequestURI()
        );
        return ResponseEntity.badRequest().body(error);
    }

    // 处理资源未找到异常
    @ExceptionHandler(ResourceNotFoundException.class)
    public ResponseEntity<ErrorResponse> handleResourceNotFoundException(
            ResourceNotFoundException ex,
            HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
                HttpStatus.NOT_FOUND.value(),
                "NOT_FOUND",
                ex.getMessage(),
                request.getRequestURI()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    // 处理验证异常
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<ErrorResponse> handleValidationException(
            MethodArgumentNotValidException ex,
            HttpServletRequest request) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        
        ErrorResponse error = new ErrorResponse(
                HttpStatus.BAD_REQUEST.value(),
                "VALIDATION_ERROR",
                "请求参数验证失败",
                request.getRequestURI()
        );
        error.setErrors(errors);
        return ResponseEntity.badRequest().body(error);
    }

    // 处理绑定异常
    @ExceptionHandler(BindException.class)
    public ResponseEntity<ErrorResponse> handleBindException(
            BindException ex,
            HttpServletRequest request) {
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        
        ErrorResponse error = new ErrorResponse(
                HttpStatus.BAD_REQUEST.value(),
                "BIND_ERROR",
                "参数绑定失败",
                request.getRequestURI()
        );
        error.setErrors(errors);
        return ResponseEntity.badRequest().body(error);
    }

    // 处理 IllegalArgumentException
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<ErrorResponse> handleIllegalArgumentException(
            IllegalArgumentException ex,
            HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
                HttpStatus.BAD_REQUEST.value(),
                "ILLEGAL_ARGUMENT",
                ex.getMessage(),
                request.getRequestURI()
        );
        return ResponseEntity.badRequest().body(error);
    }

    // 处理所有未捕获的异常
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleGenericException(
            Exception ex,
            HttpServletRequest request) {
        ErrorResponse error = new ErrorResponse(
                HttpStatus.INTERNAL_SERVER_ERROR.value(),
                "INTERNAL_ERROR",
                "服务器内部错误",
                request.getRequestURI()
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}

// 使用异常处理器的控制器
@RestController
@RequestMapping("/api/exceptions")
public class ExceptionTestController {

    @GetMapping("/business")
    public void businessException() {
        throw new BusinessException("INVALID_OPERATION", "无效的操作");
    }

    @GetMapping("/not-found")
    public void notFoundException() {
        throw new ResourceNotFoundException("资源不存在");
    }

    @GetMapping("/illegal")
    public void illegalArgumentException() {
        throw new IllegalArgumentException("参数非法");
    }
}
```

### 文件上传下载
```java
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import org.springframework.http.*;
import org.springframework.core.io.Resource;
import org.springframework.core.io.UrlResource;
import org.springframework.util.StringUtils;
import java.io.IOException;
import java.net.MalformedURLException;
import java.nio.file.*;
import java.util.*;

@RestController
@RequestMapping("/api/files")
public class FileController {

    private final Path fileStorageLocation = Paths.get("uploads").toAbsolutePath().normalize();

    public FileController() {
        try {
            Files.createDirectories(this.fileStorageLocation);
        } catch (Exception ex) {
            throw new RuntimeException("Could not create the directory where the uploaded files will be stored.", ex);
        }
    }

    // 单文件上传
    @PostMapping("/upload")
    public ResponseEntity<Map<String, Object>> uploadFile(@RequestParam("file") MultipartFile file) {
        String fileName = StringUtils.cleanPath(file.getOriginalFilename());
        
        try {
            if (fileName.contains("..")) {
                throw new RuntimeException("Filename contains invalid path sequence " + fileName);
            }
            
            Path targetLocation = this.fileStorageLocation.resolve(fileName);
            Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);
            
            Map<String, Object> response = new HashMap<>();
            response.put("fileName", fileName);
            response.put("fileSize", file.getSize());
            response.put("contentType", file.getContentType());
            response.put("url", "/api/files/download/" + fileName);
            
            return ResponseEntity.ok(response);
        } catch (IOException ex) {
            throw new RuntimeException("Could not store file " + fileName + ". Please try again!", ex);
        }
    }

    // 多文件上传
    @PostMapping("/upload/multiple")
    public ResponseEntity<List<Map<String, Object>>> uploadMultipleFiles(
            @RequestParam("files") MultipartFile[] files) {
        List<Map<String, Object>> responses = new ArrayList<>();
        
        for (MultipartFile file : files) {
            String fileName = StringUtils.cleanPath(file.getOriginalFilename());
            
            try {
                Path targetLocation = this.fileStorageLocation.resolve(fileName);
                Files.copy(file.getInputStream(), targetLocation, StandardCopyOption.REPLACE_EXISTING);
                
                Map<String, Object> response = new HashMap<>();
                response.put("fileName", fileName);
                response.put("fileSize", file.getSize());
                responses.add(response);
            } catch (IOException ex) {
                throw new RuntimeException("Could not store file " + fileName + ". Please try again!", ex);
            }
        }
        
        return ResponseEntity.ok(responses);
    }

    // 文件下载
    @GetMapping("/download/{fileName:.+}")
    public ResponseEntity<Resource> downloadFile(@PathVariable String fileName) {
        try {
            Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
            Resource resource = new UrlResource(filePath.toUri());
            
            if (resource.exists() && resource.isReadable()) {
                return ResponseEntity.ok()
                        .contentType(MediaType.parseMediaType("application/octet-stream"))
                        .header(HttpHeaders.CONTENT_DISPOSITION, 
                                "attachment; filename=\"" + resource.getFilename() + "\"")
                        .body(resource);
            } else {
                throw new RuntimeException("File not found " + fileName);
            }
        } catch (MalformedURLException ex) {
            throw new RuntimeException("File not found " + fileName, ex);
        }
    }

    // 显示图片
    @GetMapping("/image/{fileName:.+}")
    public ResponseEntity<Resource> showImage(@PathVariable String fileName) {
        try {
            Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
            Resource resource = new UrlResource(filePath.toUri());
            
            if (resource.exists() && resource.isReadable()) {
                String contentType = "image/jpeg";
                if (fileName.endsWith(".png")) {
                    contentType = "image/png";
                } else if (fileName.endsWith(".gif")) {
                    contentType = "image/gif";
                }
                
                return ResponseEntity.ok()
                        .contentType(MediaType.parseMediaType(contentType))
                        .body(resource);
            } else {
                return ResponseEntity.notFound().build();
            }
        } catch (MalformedURLException ex) {
            return ResponseEntity.notFound().build();
        }
    }

    // 删除文件
    @DeleteMapping("/{fileName:.+}")
    public ResponseEntity<Void> deleteFile(@PathVariable String fileName) {
        try {
            Path filePath = this.fileStorageLocation.resolve(fileName).normalize();
            Files.deleteIfExists(filePath);
            return ResponseEntity.noContent().build();
        } catch (IOException ex) {
            throw new RuntimeException("Could not delete file " + fileName, ex);
        }
    }

    // 文件列表
    @GetMapping("/list")
    public ResponseEntity<List<Map<String, Object>>> listFiles() {
        List<Map<String, Object>> files = new ArrayList<>();
        
        try {
            Files.walk(this.fileStorageLocation, 1)
                    .filter(Files::isRegularFile)
                    .forEach(path -> {
                        try {
                            Map<String, Object> fileInfo = new HashMap<>();
                            fileInfo.put("fileName", path.getFileName().toString());
                            fileInfo.put("fileSize", Files.size(path));
                            fileInfo.put("lastModified", Files.getLastModifiedTime(path).toString());
                            files.add(fileInfo);
                        } catch (IOException e) {
                            throw new RuntimeException(e);
                        }
                    });
        } catch (IOException ex) {
            throw new RuntimeException("Could not list files", ex);
        }
        
        return ResponseEntity.ok(files);
    }
}
```

### 拦截器配置
```java
import org.springframework.web.servlet.HandlerInterceptor;
import org.springframework.web.servlet.ModelAndView;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.*;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

// 自定义拦截器
public class LoggingInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("Request URI: " + request.getRequestURI());
        System.out.println("Request Method: " + request.getMethod());
        System.out.println("Request Time: " + System.currentTimeMillis());
        return true; // 继续处理
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("Response Status: " + response.getStatus());
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("Request Completed: " + request.getRequestURI());
    }
}

// 认证拦截器
public class AuthInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        String token = request.getHeader("Authorization");
        
        if (token == null || !isValidToken(token)) {
            response.setStatus(HttpServletResponse.SC_UNAUTHORIZED);
            response.setContentType("application/json");
            response.getWriter().write("{\"error\": \"Unauthorized\"}");
            return false;
        }
        
        return true;
    }

    private boolean isValidToken(String token) {
        return "valid-token".equals(token);
    }
}

// MVC 配置类
@Configuration
public class WebMvcConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        // 添加日志拦截器
        registry.addInterceptor(new LoggingInterceptor())
                .addPathPatterns("/**")
                .excludePathPatterns("/static/**", "/error");
        
        // 添加认证拦截器
        registry.addInterceptor(new AuthInterceptor())
                .addPathPatterns("/api/**")
                .excludePathPatterns("/api/public/**", "/api/auth/**");
    }

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        // 静态资源处理
        registry.addResourceHandler("/uploads/**")
                .addResourceLocations("file:uploads/");
        
        registry.addResourceHandler("/static/**")
                .addResourceLocations("classpath:/static/");
    }

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        // CORS 配置
        registry.addMapping("/**")
                .allowedOrigins("*")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .maxAge(3600);
    }

    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        // 视图控制器
        registry.addViewController("/").setViewName("index");
        registry.addViewController("/home").setViewName("home");
    }
}
```

## 4. 官方示例代码（可运行）

### 完整的用户管理 API
```java
// ==================== 用户实体类 ====================
import javax.persistence.*;
import javax.validation.constraints.*;

@Entity
@Table(name = "users")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @NotBlank(message = "用户名不能为空")
    @Size(min = 3, max = 50, message = "用户名长度必须在 3-50 之间")
    @Column(nullable = false, unique = true)
    private String username;

    @NotBlank(message = "邮箱不能为空")
    @Email(message = "邮箱格式不正确")
    @Column(nullable = false, unique = true)
    private String email;

    @NotBlank(message = "密码不能为空")
    @Size(min = 6, message = "密码至少 6 个字符")
    @Column(nullable = false)
    private String password;

    @Pattern(regexp = "^1[3-9]\\d{9}$", message = "手机号格式不正确")
    private String phone;

    @Min(value = 0, message = "年龄不能小于 0")
    @Max(value = 150, message = "年龄不能大于 150")
    private Integer age;

    @Column(name = "created_at")
    private java.time.LocalDateTime createdAt;

    @Column(name = "updated_at")
    private java.time.LocalDateTime updatedAt;

    @PrePersist
    protected void onCreate() {
        createdAt = java.time.LocalDateTime.now();
        updatedAt = java.time.LocalDateTime.now();
    }

    @PreUpdate
    protected void onUpdate() {
        updatedAt = java.time.LocalDateTime.now();
    }

    // getters and setters
    public Long getId() { return id; }
    public void setId(Long id) { this.id = id; }
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
    public String getPhone() { return phone; }
    public void setPhone(String phone) { this.phone = phone; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
    public java.time.LocalDateTime getCreatedAt() { return createdAt; }
    public java.time.LocalDateTime getUpdatedAt() { return updatedAt; }
}

// ==================== 用户 DTO ====================
class UserDTO {
    private Long id;
    private String username;
    private String email;
    private String phone;
    private Integer age;
    private java.time.LocalDateTime createdAt;

    public static UserDTO from(User user) {
        UserDTO dto = new UserDTO();
        dto.id = user.getId();
        dto.username = user.getUsername();
        dto.email = user.getEmail();
        dto.phone = user.getPhone();
        dto.age = user.getAge();
        dto.createdAt = user.getCreatedAt();
        return dto;
    }

    // getters
    public Long getId() { return id; }
    public String getUsername() { return username; }
    public String getEmail() { return email; }
    public String getPhone() { return phone; }
    public Integer getAge() { return age; }
    public java.time.LocalDateTime getCreatedAt() { return createdAt; }
}

// ==================== 创建用户请求 DTO ====================
class CreateUserRequest {
    @NotBlank(message = "用户名不能为空")
    @Size(min = 3, max = 50)
    private String username;

    @NotBlank(message = "邮箱不能为空")
    @Email
    private String email;

    @NotBlank(message = "密码不能为空")
    @Size(min = 6)
    private String password;

    private String phone;
    private Integer age;

    // getters and setters
    public String getUsername() { return username; }
    public void setUsername(String username) { this.username = username; }
    public String getEmail() { return email; }
    public void setEmail(String email) { this.email = email; }
    public String getPassword() { return password; }
    public void setPassword(String password) { this.password = password; }
    public String getPhone() { return phone; }
    public void setPhone(String phone) { this.phone = phone; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
}

// ==================== 更新用户请求 DTO ====================
class UpdateUserRequest {
    private String phone;
    private Integer age;

    // getters and setters
    public String getPhone() { return phone; }
    public void setPhone(String phone) { this.phone = phone; }
    public Integer getAge() { return age; }
    public void setAge(Integer age) { this.age = age; }
}

// ==================== 自定义异常 ====================
class UserNotFoundException extends RuntimeException {
    public UserNotFoundException(Long id) {
        super("User not found with id: " + id);
    }

    public UserNotFoundException(String email) {
        super("User not found with email: " + email);
    }
}

class UserAlreadyExistsException extends RuntimeException {
    public UserAlreadyExistsException(String message) {
        super(message);
    }
}

// ==================== 用户 Service ====================
import org.springframework.stereotype.Service;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import java.util.Optional;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public Page<UserDTO> findAll(Pageable pageable) {
        return userRepository.findAll(pageable).map(UserDTO::from);
    }

    public UserDTO findById(Long id) {
        return userRepository.findById(id)
                .map(UserDTO::from)
                .orElseThrow(() -> new UserNotFoundException(id));
    }

    public UserDTO create(CreateUserRequest request) {
        if (userRepository.existsByUsername(request.getUsername())) {
            throw new UserAlreadyExistsException("Username already exists: " + request.getUsername());
        }

        if (userRepository.existsByEmail(request.getEmail())) {
            throw new UserAlreadyExistsException("Email already exists: " + request.getEmail());
        }

        User user = new User();
        user.setUsername(request.getUsername());
        user.setEmail(request.getEmail());
        user.setPassword(request.getPassword()); // 实际应用中应该加密
        user.setPhone(request.getPhone());
        user.setAge(request.getAge());

        User savedUser = userRepository.save(user);
        return UserDTO.from(savedUser);
    }

    public UserDTO update(Long id, UpdateUserRequest request) {
        User user = userRepository.findById(id)
                .orElseThrow(() -> new UserNotFoundException(id));

        if (request.getPhone() != null) {
            user.setPhone(request.getPhone());
        }
        if (request.getAge() != null) {
            user.setAge(request.getAge());
        }

        User updatedUser = userRepository.save(user);
        return UserDTO.from(updatedUser);
    }

    public void delete(Long id) {
        if (!userRepository.existsById(id)) {
            throw new UserNotFoundException(id);
        }
        userRepository.deleteById(id);
    }

    public UserDTO findByEmail(String email) {
        return userRepository.findByEmail(email)
                .map(UserDTO::from)
                .orElseThrow(() -> new UserNotFoundException(email));
    }
}

// ==================== 用户 Repository ====================
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import java.util.Optional;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    boolean existsByUsername(String username);
    boolean existsByEmail(String email);
    Optional<User> findByEmail(String email);
    Optional<User> findByUsername(String username);
}

// ==================== 全局异常处理器 ====================
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.validation.FieldError;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.*;
import java.util.HashMap;
import java.util.Map;
import java.time.LocalDateTime;

class ApiError {
    private LocalDateTime timestamp;
    private int status;
    private String error;
    private String message;

    public ApiError(int status, String error, String message) {
        this.timestamp = LocalDateTime.now();
        this.status = status;
        this.error = error;
        this.message = message;
    }

    public LocalDateTime getTimestamp() { return timestamp; }
    public int getStatus() { return status; }
    public String getError() { return error; }
    public String getMessage() { return message; }
}

@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(UserNotFoundException.class)
    public ResponseEntity<ApiError> handleUserNotFoundException(UserNotFoundException ex) {
        ApiError error = new ApiError(
                HttpStatus.NOT_FOUND.value(),
                "NOT_FOUND",
                ex.getMessage()
        );
        return ResponseEntity.status(HttpStatus.NOT_FOUND).body(error);
    }

    @ExceptionHandler(UserAlreadyExistsException.class)
    public ResponseEntity<ApiError> handleUserAlreadyExistsException(UserAlreadyExistsException ex) {
        ApiError error = new ApiError(
                HttpStatus.CONFLICT.value(),
                "CONFLICT",
                ex.getMessage()
        );
        return ResponseEntity.status(HttpStatus.CONFLICT).body(error);
    }

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<Map<String, Object>> handleValidationException(MethodArgumentNotValidException ex) {
        Map<String, Object> response = new HashMap<>();
        response.put("timestamp", LocalDateTime.now());
        response.put("status", HttpStatus.BAD_REQUEST.value());
        response.put("error", "VALIDATION_ERROR");
        
        Map<String, String> errors = new HashMap<>();
        ex.getBindingResult().getAllErrors().forEach((error) -> {
            String fieldName = ((FieldError) error).getField();
            String errorMessage = error.getDefaultMessage();
            errors.put(fieldName, errorMessage);
        });
        response.put("errors", errors);
        
        return ResponseEntity.badRequest().body(response);
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<ApiError> handleGenericException(Exception ex) {
        ApiError error = new ApiError(
                HttpStatus.INTERNAL_SERVER_ERROR.value(),
                "INTERNAL_ERROR",
                "An unexpected error occurred"
        );
        return ResponseEntity.status(HttpStatus.INTERNAL_SERVER_ERROR).body(error);
    }
}

// ==================== 用户 Controller ====================
import org.springframework.web.bind.annotation.*;
import org.springframework.http.ResponseEntity;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.web.PageableDefault;
import org.springframework.web.util.UriComponentsBuilder;
import javax.validation.Valid;
import java.net.URI;

@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping
    public ResponseEntity<Page<UserDTO>> getAllUsers(
            @PageableDefault(size = 10, sort = "createdAt") Pageable pageable) {
        Page<UserDTO> users = userService.findAll(pageable);
        return ResponseEntity.ok(users);
    }

    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUserById(@PathVariable Long id) {
        UserDTO user = userService.findById(id);
        return ResponseEntity.ok(user);
    }

    @PostMapping
    public ResponseEntity<UserDTO> createUser(
            @Valid @RequestBody CreateUserRequest request,
            UriComponentsBuilder uriBuilder) {
        UserDTO createdUser = userService.create(request);
        URI location = uriBuilder.path("/api/users/{id}")
                .buildAndExpand(createdUser.getId())
                .toUri();
        return ResponseEntity.created(location).body(createdUser);
    }

    @PutMapping("/{id}")
    public ResponseEntity<UserDTO> updateUser(
            @PathVariable Long id,
            @Valid @RequestBody UpdateUserRequest request) {
        UserDTO updatedUser = userService.update(id, request);
        return ResponseEntity.ok(updatedUser);
    }

    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.delete(id);
        return ResponseEntity.noContent().build();
    }
}

// ==================== 主应用类 ====================
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class UserManagementApplication {
    public static void main(String[] args) {
        SpringApplication.run(UserManagementApplication.class, args);
    }
}
```

## 5. 官方注意事项 / 最佳实践

### REST API 设计最佳实践
1. **使用正确的 HTTP 方法**：GET（查询）、POST（创建）、PUT（更新）、DELETE（删除）、PATCH（部分更新）
2. **使用合适的 HTTP 状态码**：200（成功）、201（创建成功）、204（无内容）、400（请求错误）、401（未认证）、403（无权限）、404（不存在）、500（服务器错误）
3. **使用名词复数**：如 /api/users 而非 /api/user
4. **使用层级资源**：如 /api/users/{id}/orders
5. **使用查询参数过滤、排序、分页**
6. **版本控制**：如 /api/v1/users
7. **请求和响应使用 JSON**：统一数据格式
8. **提供清晰的错误信息**：包含错误码和错误描述

### 控制器最佳实践
1. **保持控制器简洁**：控制器只负责请求/响应处理，业务逻辑放在 Service 层
2. **使用 DTO**：不要直接暴露实体类，使用数据传输对象
3. **参数验证**：使用 @Valid 和 JSR-380 注解验证请求参数
4. **统一异常处理**：使用 @ControllerAdvice 统一处理异常
5. **使用 ResponseEntity**：灵活控制响应状态码、头、体
6. **避免在控制器中使用 Session**：保持无状态

### 数据验证最佳实践
1. **在 DTO 中验证**：使用 @NotBlank、@NotNull、@Email 等注解
2. **分组验证**：使用 @Validated 和分组接口实现不同场景的验证
3. **自定义验证注解**：复杂验证逻辑创建自定义验证器
4. **验证结果处理**：使用 BindingResult 或统一异常处理
5. **服务层也验证**：不要只依赖控制器层的验证

### 异常处理最佳实践
1. **使用全局异常处理器**：@RestControllerAdvice 统一处理所有异常
2. **创建自定义异常**：业务异常继承 RuntimeException
3. **提供有意义的错误信息**：包含时间戳、状态码、错误码、错误消息
4. **处理验证异常**：MethodArgumentNotValidException 详细的字段错误
5. **区分异常类型**：业务异常、验证异常、系统异常分别处理
6. **记录异常日志**：在异常处理器中记录异常日志

### 文件上传下载最佳实践
1. **验证文件类型**：检查文件扩展名和 MIME 类型
2. **限制文件大小**：配置最大文件大小限制
3. **使用安全的文件名**：清理文件名，防止路径遍历攻击
4. **文件隔离**：上传文件存储在应用外的独立目录
5. **文件访问控制**：通过应用控制文件访问，不要直接暴露文件目录
6. **定期清理**：定期清理临时文件和过期文件
7. **使用 CDN**：大文件考虑使用 CDN

### 性能优化
1. **使用分页**：大数据量查询使用分页
2. **缓存**：频繁访问的数据使用缓存
3. **避免 N+1 查询**：使用 JOIN FETCH 或 @EntityGraph
4. **压缩响应**：启用 GZIP 压缩
5. **使用异步处理**：耗时操作使用 @Async
6. **静态资源缓存**：配置静态资源缓存策略

### 安全最佳实践
1. **认证和授权**：使用 Spring Security 保护 API
2. **输入验证**：验证所有输入，防止注入攻击
3. **HTTPS**：生产环境使用 HTTPS
4. **CORS 配置**：合理配置跨域访问
5. **敏感信息保护**：不要在日志中记录密码、token 等敏感信息
6. **CSRF 保护**：表单提交使用 CSRF 保护
7. **速率限制**：防止暴力攻击和 DDoS

## 6. 官方文档参考链接

- Spring Boot Web MVC 官方文档：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web
- Spring MVC 官方文档：https://docs.spring.io/spring-framework/docs/current/reference/html/web.html
- @RestController：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web.servlet.spring-mvc.annotation.rest-controller
- ResponseEntity：https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/http/ResponseEntity.html
- 验证：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#io.validation
- 异常处理：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#web.servlet.error-handling
- 文件上传：https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto.servlet.multipart
- CORS：https://docs.spring.io/spring-framework/docs/current/reference/html/web.html#mvc-cors
- Spring Boot GitHub：https://github.com/spring-projects/spring-boot
