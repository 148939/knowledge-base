# 检索索引：本文档收录【Gin】【Web 框架】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Gin 是一个用 Go (Golang) 编写的 HTTP Web 框架。它具有类似 Martini 的 API，但性能更好，速度提升高达 40 倍。Gin 是一个轻量级、高性能的 Web 框架，提供了路由、中间件、参数绑定、JSON/XML 渲染、文件上传等功能，非常适合构建 RESTful API 和 Web 应用。

## 2. 核心知识点（结构化层级）

### 2.1 快速开始

- 安装 Gin：go get -u github.com/gin-gonic/gin
- 基础路由：GET、POST、PUT、DELETE、PATCH
- 路由参数：路径参数、查询参数
- 启动服务器：Run()

### 2.2 路由

- 基础路由：GET、POST、PUT、DELETE、PATCH、OPTIONS、HEAD
- 路由组：Group()、分组路由
- 路径参数：:param、*wildcard
- 查询参数：Query()、DefaultQuery()
- 表单参数：PostForm()、DefaultPostForm()
- 路由命名：Named routes

### 2.3 请求处理

- 参数绑定：ShouldBind、ShouldBindJSON、ShouldBindQuery、ShouldBindForm
- 验证：结构体验证、自定义验证
- 文件上传：FormFile、MultipartForm
- 请求头：GetHeader()

### 2.4 响应处理

- JSON 响应：JSON、IndentedJSON、SecureJSON
- XML 响应：XML、IndentedXML
- 字符串响应：String
- HTML 响应：HTML、LoadHTMLFiles、LoadHTMLGlob
- 文件响应：File、FileAttachment
- 重定向：Redirect

### 2.5 中间件

- 全局中间件：Use()
- 路由组中间件
- 单个路由中间件
- 内置中间件：Logger、Recovery
- 自定义中间件
- 中间件链

### 2.6 错误处理

- 错误响应：AbortWithError、AbortWithStatusJSON
- 自定义错误处理
- 全局错误处理中间件

### 2.7 会话与 Cookie

- Cookie：SetCookie、Cookie
- Session：使用 gin-contrib/sessions

### 2.8 模板渲染

- HTML 模板：LoadHTMLFiles、LoadHTMLGlob
- 模板函数：SetFuncMap
- 自定义模板渲染

## 3. 官方标准语法 / API

```go
package main

import (
	"net/http"
	"github.com/gin-gonic/gin"
)

func main() {
	// 创建 Gin 引擎
	r := gin.Default()
	
	// 基础路由
	r.GET("/", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "Hello Gin!",
		})
	})
	
	// 路径参数
	r.GET("/users/:id", func(c *gin.Context) {
		id := c.Param("id")
		c.JSON(http.StatusOK, gin.H{"id": id})
	})
	
	// 查询参数
	r.GET("/welcome", func(c *gin.Context) {
		firstname := c.DefaultQuery("firstname", "Guest")
		lastname := c.Query("lastname")
		c.String(http.StatusOK, "Hello %s %s", firstname, lastname)
	})
	
	// POST 表单参数
	r.POST("/form", func(c *gin.Context) {
		message := c.PostForm("message")
		nick := c.DefaultPostForm("nick", "anonymous")
		c.JSON(http.StatusOK, gin.H{
			"status":  "posted",
			"message": message,
			"nick":    nick,
		})
	})
	
	// JSON 参数绑定
	type Login struct {
		User     string `json:"user" binding:"required"`
		Password string `json:"password" binding:"required"`
	}
	
	r.POST("/login", func(c *gin.Context) {
		var json Login
		if err := c.ShouldBindJSON(&json); err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			return
		}
		c.JSON(http.StatusOK, gin.H{"status": "success"})
	})
	
	// 路由组
	v1 := r.Group("/v1")
	{
		v1.GET("/users", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"message": "v1 users"})
		})
		v1.POST("/users", func(c *gin.Context) {
			c.JSON(http.StatusOK, gin.H{"message": "create user"})
		})
	}
	
	// 启动服务器
	r.Run(":8080")
}
```

## 4. 官方示例代码（可运行）

```go
package main

import (
	"fmt"
	"net/http"
	"path/filepath"
	"time"

	"github.com/gin-gonic/gin"
)

// User 模型
type User struct {
	ID        int       `json:"id"`
	Username  string    `json:"username" binding:"required"`
	Email     string    `json:"email" binding:"required,email"`
	Password  string    `json:"password,omitempty" binding:"required,min=6"`
	CreatedAt time.Time `json:"created_at"`
}

// Article 模型
type Article struct {
	ID      int    `json:"id"`
	Title   string `json:"title" binding:"required"`
	Content string `json:"content" binding:"required"`
	Author  string `json:"author"`
}

var (
	users    = make(map[int]*User)
	articles = make(map[int]*Article)
	nextUserID  = 1
	nextArticleID = 1
)

// AuthMiddleware 认证中间件
func AuthMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		token := c.GetHeader("Authorization")
		if token == "" {
			c.AbortWithStatusJSON(http.StatusUnauthorized, gin.H{"error": "未授权"})
			return
		}
		// 这里应该验证 token
		c.Set("user_id", 1)
		c.Next()
	}
}

// LoggerMiddleware 日志中间件
func LoggerMiddleware() gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()
		path := c.Request.URL.Path
		
		c.Next()
		
		latency := time.Since(start)
		status := c.Writer.Status()
		fmt.Printf("[GIN] %v | %3d | %13v | %s\n",
			time.Now().Format("2006/01/02 - 15:04:05"),
			status,
			latency,
			path,
		)
	}
}

func main() {
	// 创建 Gin 引擎
	r := gin.Default()
	
	// 自定义中间件
	r.Use(LoggerMiddleware())
	
	// 加载 HTML 模板
	r.LoadHTMLGlob("templates/*")
	
	// 静态文件
	r.Static("/static", "./static")
	
	// 健康检查
	r.GET("/health", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"status": "ok",
			"time":   time.Now(),
		})
	})
	
	// API 路由组
	api := r.Group("/api")
	{
		// 用户路由
		usersGroup := api.Group("/users")
		{
			// 创建用户
			usersGroup.POST("", func(c *gin.Context) {
				var user User
				if err := c.ShouldBindJSON(&user); err != nil {
					c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
					return
				}
				
				user.ID = nextUserID
				user.CreatedAt = time.Now()
				users[user.ID] = &user
				nextUserID++
				
				user.Password = ""
				c.JSON(http.StatusCreated, user)
			})
			
			// 获取用户列表
			usersGroup.GET("", func(c *gin.Context) {
				var userList []*User
				for _, u := range users {
					u.Password = ""
					userList = append(userList, u)
				}
				c.JSON(http.StatusOK, userList)
			})
			
			// 获取单个用户
			usersGroup.GET("/:id", func(c *gin.Context) {
				id := 0
				fmt.Sscanf(c.Param("id"), "%d", &id)
				
				if user, exists := users[id]; exists {
					user.Password = ""
					c.JSON(http.StatusOK, user)
				} else {
					c.JSON(http.StatusNotFound, gin.H{"error": "用户不存在"})
				}
			})
		}
		
		// 文章路由（需要认证）
		articlesGroup := api.Group("/articles")
		articlesGroup.Use(AuthMiddleware())
		{
			// 创建文章
			articlesGroup.POST("", func(c *gin.Context) {
				var article Article
				if err := c.ShouldBindJSON(&article); err != nil {
					c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
					return
				}
				
				userID, _ := c.Get("user_id")
				article.ID = nextArticleID
				article.Author = fmt.Sprintf("user_%d", userID)
				articles[article.ID] = &article
				nextArticleID++
				
				c.JSON(http.StatusCreated, article)
			})
			
			// 获取文章列表
			articlesGroup.GET("", func(c *gin.Context) {
				var articleList []*Article
				for _, a := range articles {
					articleList = append(articleList, a)
				}
				c.JSON(http.StatusOK, articleList)
			})
			
			// 获取单个文章
			articlesGroup.GET("/:id", func(c *gin.Context) {
				id := 0
				fmt.Sscanf(c.Param("id"), "%d", &id)
				
				if article, exists := articles[id]; exists {
					c.JSON(http.StatusOK, article)
				} else {
					c.JSON(http.StatusNotFound, gin.H{"error": "文章不存在"})
				}
			})
		}
	}
	
	// 文件上传
	r.POST("/upload", func(c *gin.Context) {
		file, err := c.FormFile("file")
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
			return
		}
		
		filename := filepath.Base(file.Filename)
		if err := c.SaveUploadedFile(file, filename); err != nil {
			c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
			return
		}
		
		c.JSON(http.StatusOK, gin.H{
			"message": "文件上传成功",
			"filename": filename,
		})
	})
	
	// HTML 渲染
	r.GET("/index", func(c *gin.Context) {
		c.HTML(http.StatusOK, "index.html", gin.H{
			"title": "Gin 示例",
		})
	})
	
	// 启动服务器
	fmt.Println("服务器启动在 http://localhost:8080")
	r.Run(":8080")
}
```

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head>
    <title>{{ .title }}</title>
</head>
<body>
    <h1>{{ .title }}</h1>
    <p>欢迎使用 Gin Web 框架！</p>
</body>
</html>
```

## 5. 官方注意事项 / 最佳实践

1. **性能优化**
   - 使用路由组组织相关路由
   - 合理使用中间件，避免过度使用
   - 参数绑定使用 ShouldBind 而非 Bind
   - 生产环境使用 gin.ReleaseMode

2. **安全实践**
   - 使用 HTTPS
   - 验证所有输入数据
   - 使用结构化日志
   - 防止 SQL 注入、XSS、CSRF

3. **错误处理**
   - 统一错误响应格式
   - 使用中间件处理全局错误
   - 提供有意义的错误信息
   - 记录详细的错误日志

4. **代码组织**
   - 分离路由、处理器、模型
   - 使用依赖注入
   - 合理使用中间件链
   - 编写可测试的代码

## 6. 官方文档参考链接

- GitHub 仓库：https://github.com/gin-gonic/gin
- 官方文档：https://gin-gonic.com/docs/
- 示例代码：https://github.com/gin-gonic/examples
- Gin 中文文档：https://gin-gonic.com/zh-cn/docs/
