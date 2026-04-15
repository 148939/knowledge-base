# 检索索引：本文档收录【Go】【基础语法】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Go（又称 Golang）是 Google 开发的一种静态强类型、编译型、并发型，并具有垃圾回收功能的编程语言。Go 的设计目标是结合静态语言的安全性和动态语言的开发效率，具有简洁、高效、并发安全等特点，广泛应用于云原生、微服务、DevOps 等领域。

## 2. 核心知识点（结构化层级）

### 2.1 基础语法

- 变量声明：var、:= 短变量声明
- 数据类型：bool、string、int、float、complex、array、slice、map、struct、interface
- 常量：const、iota
- 运算符：算术、比较、逻辑、位、赋值
- 控制流：if、for、switch、select、defer

### 2.2 函数

- 函数声明：func
- 参数与返回值：多返回值、命名返回值
- 可变参数：...
- 匿名函数与闭包
- defer 语句：延迟执行
- panic 与 recover：异常处理

### 2.3 数据结构

- 数组（Array）：固定长度
- 切片（Slice）：动态数组、长度与容量、append、copy
- 映射（Map）：键值对、make、delete
- 结构体（Struct）：字段、方法、嵌入

### 2.4 指针与接口

- 指针：*、&、new
- 接口（Interface）：方法集、类型断言、类型选择
- 空接口：interface{}
- 错误处理：error 接口

### 2.5 并发编程

- Goroutine：轻量级线程、go 关键字
- Channel：通道、无缓冲/有缓冲、range、close
- Select：多路复用
- Mutex：互斥锁、sync 包
- WaitGroup：等待组

### 2.6 包与模块

- 包（Package）：package、import
- 可见性：大写字母开头公开
- 模块（Module）：go mod、go.sum
- 依赖管理：go get、go mod tidy

## 3. 官方标准语法 / API

```go
package main

import (
	"fmt"
	"time"
)

// 变量声明
var globalVar int = 100

func main() {
	// 变量声明
	var a int = 10
	var b = 20
	c := 30
	fmt.Println(a, b, c)
	
	// 常量
	const Pi = 3.14159
	const (
		StatusOK = 200
		StatusNotFound = 404
	)
	
	// 基本数据类型
	var isActive bool = true
	var name string = "Go"
	var age int = 25
	var score float64 = 98.5
	
	// 数组
	var arr [5]int
	arr[0] = 1
	arr2 := [3]int{1, 2, 3}
	
	// 切片
	slice := []int{1, 2, 3}
	slice = append(slice, 4)
	subSlice := slice[1:3]
	
	// 映射
	m := make(map[string]int)
	m["one"] = 1
	m["two"] = 2
	delete(m, "two")
	
	// 结构体
	type Person struct {
		Name string
		Age  int
	}
	p := Person{Name: "Alice", Age: 30}
	
	// 指针
	x := 10
	ptr := &x
	*ptr = 20
	
	// 控制流
	if x > 0 {
		fmt.Println("Positive")
	}
	
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}
	
	for key, value := range m {
		fmt.Println(key, value)
	}
	
	// 函数调用
	result := add(5, 3)
	sum, diff := addAndSub(10, 5)
	
	// defer
	defer fmt.Println("Deferred 1")
	defer fmt.Println("Deferred 2")
	
	// Goroutine
	go sayHello()
	
	// Channel
	ch := make(chan int)
	go sendData(ch)
	received := <-ch
}

// 函数定义
func add(a, b int) int {
	return a + b
}

// 多返回值
func addAndSub(a, b int) (int, int) {
	return a + b, a - b
}

// 命名返回值
func divide(a, b int) (result int, err error) {
	if b == 0 {
		return 0, fmt.Errorf("division by zero")
	}
	return a / b, nil
}

// Goroutine 函数
func sayHello() {
	time.Sleep(1 * time.Second)
	fmt.Println("Hello from Goroutine!")
}

func sendData(ch chan<- int) {
	ch <- 42
	close(ch)
}
```

## 4. 官方示例代码（可运行）

```go
package main

import (
	"fmt"
	"sync"
	"time"
)

// 示例1: 结构体与方法
type Rectangle struct {
	Width  float64
	Height float64
}

func (r Rectangle) Area() float64 {
	return r.Width * r.Height
}

func (r *Rectangle) Scale(factor float64) {
	r.Width *= factor
	r.Height *= factor
}

// 示例2: 接口
type Shape interface {
	Area() float64
}

type Circle struct {
	Radius float64
}

func (c Circle) Area() float64 {
	return 3.14159 * c.Radius * c.Radius
}

func PrintArea(s Shape) {
	fmt.Printf("Area: %.2f\n", s.Area())
}

// 示例3: 错误处理
type MyError struct {
	When time.Time
	What string
}

func (e *MyError) Error() string {
	return fmt.Sprintf("at %v, %s", e.When, e.What)
}

func run() error {
	return &MyError{
		time.Now(),
		"something went wrong",
	}
}

// 示例4: 并发编程 - Goroutine与Channel
func worker(id int, jobs <-chan int, results chan<- int) {
	for j := range jobs {
		fmt.Printf("Worker %d started job %d\n", id, j)
		time.Sleep(time.Second)
		fmt.Printf("Worker %d finished job %d\n", id, j)
		results <- j * 2
	}
}

// 示例5: 并发编程 - Mutex
type SafeCounter struct {
	mu sync.Mutex
	v  map[string]int
}

func (c *SafeCounter) Inc(key string) {
	c.mu.Lock()
	c.v[key]++
	c.mu.Unlock()
}

func (c *SafeCounter) Value(key string) int {
	c.mu.Lock()
	defer c.mu.Unlock()
	return c.v[key]
}

// 示例6: 切片与映射操作
func sliceOperations() {
	// 创建切片
	numbers := []int{1, 2, 3, 4, 5}
	fmt.Println("Original:", numbers)
	
	// 追加元素
	numbers = append(numbers, 6, 7)
	fmt.Println("After append:", numbers)
	
	// 切片
	sub := numbers[2:5]
	fmt.Println("Sub-slice:", sub)
	
	// 复制
	copied := make([]int, len(numbers))
	copy(copied, numbers)
	fmt.Println("Copied:", copied)
}

func mapOperations() {
	// 创建映射
	scores := make(map[string]int)
	scores["Alice"] = 95
	scores["Bob"] = 87
	scores["Charlie"] = 92
	
	// 遍历
	fmt.Println("Scores:")
	for name, score := range scores {
		fmt.Printf("%s: %d\n", name, score)
	}
	
	// 检查存在
	score, exists := scores["Dave"]
	if exists {
		fmt.Println("Dave's score:", score)
	} else {
		fmt.Println("Dave not found")
	}
}

// 主函数
func main() {
	fmt.Println("=== 结构体与方法 ===")
	rect := Rectangle{Width: 10, Height: 5}
	fmt.Printf("Area: %.2f\n", rect.Area())
	rect.Scale(2)
	fmt.Printf("After scaling - Area: %.2f\n", rect.Area())
	
	fmt.Println("\n=== 接口 ===")
	rect2 := Rectangle{Width: 3, Height: 4}
	circle := Circle{Radius: 5}
	PrintArea(rect2)
	PrintArea(circle)
	
	fmt.Println("\n=== 错误处理 ===")
	if err := run(); err != nil {
		fmt.Println(err)
	}
	
	fmt.Println("\n=== 并发 - Worker Pool ===")
	const numJobs = 5
	const numWorkers = 3
	
	jobs := make(chan int, numJobs)
	results := make(chan int, numJobs)
	
	for w := 1; w <= numWorkers; w++ {
		go worker(w, jobs, results)
	}
	
	for j := 1; j <= numJobs; j++ {
		jobs <- j
	}
	close(jobs)
	
	for a := 1; a <= numJobs; a++ {
		fmt.Printf("Result: %d\n", <-results)
	}
	
	fmt.Println("\n=== 并发 - SafeCounter ===")
	c := SafeCounter{v: make(map[string]int)}
	for i := 0; i < 1000; i++ {
		go c.Inc("somekey")
	}
	time.Sleep(time.Second)
	fmt.Println("Counter value:", c.Value("somekey"))
	
	fmt.Println("\n=== 切片操作 ===")
	sliceOperations()
	
	fmt.Println("\n=== 映射操作 ===")
	mapOperations()
}
```

## 5. 官方注意事项 / 最佳实践

1. **代码风格**
   - 使用 gofmt 格式化代码
   - 遵循 Effective Go 指南
   - 使用有意义的变量和函数名
   - 合理使用空行和注释

2. **错误处理**
   - 始终检查并处理错误
   - 不要忽略错误返回值
   - 提供有意义的错误信息
   - 使用自定义错误类型

3. **并发编程**
   - 优先使用 Channel 而非共享内存
   - 合理使用 Goroutine，避免过度创建
   - 使用 sync 包处理同步问题
   - 注意 Goroutine 泄漏问题

4. **性能优化**
   - 合理使用切片容量，避免频繁扩容
   - 使用 sync.Pool 复用对象
   - 避免在循环中创建不必要的对象
   - 使用基准测试（benchmark）分析性能

5. **安全建议**
   - 不要信任用户输入
   - 注意内存安全问题
   - 合理使用 defer 释放资源
   - 避免 race condition

## 6. 官方文档参考链接

- 官方文档：https://go.dev/doc/
- Go 语言规范：https://go.dev/ref/spec
- Effective Go：https://go.dev/doc/effective_go
- 标准库：https://pkg.go.dev/std
- Go 博客：https://go.dev/blog/
