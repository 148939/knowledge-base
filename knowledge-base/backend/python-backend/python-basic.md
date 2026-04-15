# 检索索引：本文档收录【Python】【基础入门】知识点，内容100%来自官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Python 是一种解释型、高级、通用的编程语言。它的设计哲学强调代码的可读性，使用显著的缩进。Python 支持多种编程范式，包括结构化、面向对象和函数式编程。它具有动态类型和自动垃圾回收的特性，被广泛应用于 Web 开发、数据科学、人工智能、自动化等领域。

## 2. 核心知识点（结构化层级）

### 2.1 基础语法

- 变量与数据类型：整数、浮点数、字符串、布尔值、列表、元组、字典、集合
- 运算符：算术、比较、逻辑、赋值、成员、身份运算符
- 条件语句：if、elif、else
- 循环语句：for、while、break、continue
- 函数：def、参数、返回值、lambda 表达式
- 异常处理：try、except、finally、raise

### 2.2 数据结构

- 列表（List）：有序可变序列
- 元组（Tuple）：有序不可变序列
- 字典（Dictionary）：键值对映射
- 集合（Set）：无序不重复元素集合

### 2.3 面向对象编程

- 类与对象：class、__init__ 构造函数
- 继承与多态
- 封装与访问控制
- 特殊方法（魔术方法）

### 2.4 模块与包

- 导入模块：import、from...import
- 创建模块
- 包的组织结构

## 3. 官方标准语法 / API

```python
# 变量与数据类型
name = "Python"
age = 30
pi = 3.14159
is_active = True
numbers = [1, 2, 3, 4, 5]
person = {"name": "Alice", "age": 25}
unique_items = {1, 2, 3, 2, 1}

# 条件语句
if age >= 18:
    print("成年人")
elif age >= 13:
    print("青少年")
else:
    print("儿童")

# 循环语句
for i in range(5):
    print(i)

count = 0
while count < 5:
    print(count)
    count += 1

# 函数定义
def greet(name, greeting="Hello"):
    return f"{greeting}, {name}!"

# lambda 函数
square = lambda x: x ** 2

# 异常处理
try:
    result = 10 / 0
except ZeroDivisionError as e:
    print(f"错误: {e}")
finally:
    print("执行完毕")

# 类定义
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return f"{self.name} 说: 汪汪!"

# 模块导入
import math
from datetime import datetime
```

## 4. 官方示例代码（可运行）

```python
# 示例1: 列表操作
def list_operations():
    # 创建列表
    fruits = ["apple", "banana", "cherry"]
    
    # 添加元素
    fruits.append("orange")
    fruits.insert(1, "grape")
    
    # 访问元素
    print(f"第一个元素: {fruits[0]}")
    print(f"最后一个元素: {fruits[-1]}")
    
    # 切片
    print(f"前两个元素: {fruits[:2]}")
    
    # 列表推导式
    squares = [x ** 2 for x in range(10)]
    print(f"平方数: {squares}")
    
    return fruits

# 示例2: 字典操作
def dict_operations():
    # 创建字典
    student = {
        "name": "张三",
        "age": 20,
        "major": "计算机科学",
        "courses": ["Python", "Java", "数据结构"]
    }
    
    # 访问值
    print(f"姓名: {student['name']}")
    print(f"专业: {student.get('major', '未知')}")
    
    # 添加键值对
    student["grade"] = 3
    
    # 遍历字典
    for key, value in student.items():
        print(f"{key}: {value}")
    
    return student

# 示例3: 函数与装饰器
def timer_decorator(func):
    import time
    
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} 执行时间: {end_time - start_time:.4f} 秒")
        return result
    
    return wrapper

@timer_decorator
def calculate_factorial(n):
    if n == 0 or n == 1:
        return 1
    return n * calculate_factorial(n - 1)

@timer_decorator
def fibonacci(n):
    if n <= 1:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)

# 示例4: 文件操作
def file_operations():
    # 写入文件
    with open("example.txt", "w", encoding="utf-8") as f:
        f.write("Hello, Python!\n")
        f.write("这是一个示例文件。\n")
    
    # 读取文件
    with open("example.txt", "r", encoding="utf-8") as f:
        content = f.read()
        print("文件内容:")
        print(content)
    
    # 逐行读取
    with open("example.txt", "r", encoding="utf-8") as f:
        lines = f.readlines()
        print(f"行数: {len(lines)}")

# 示例5: 面向对象编程完整示例
class BankAccount:
    def __init__(self, account_number, owner, balance=0.0):
        self.account_number = account_number
        self.owner = owner
        self.__balance = balance  # 私有属性
    
    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount
            print(f"存款成功: +{amount}")
        else:
            print("存款金额必须大于0")
    
    def withdraw(self, amount):
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            print(f"取款成功: -{amount}")
        else:
            print("取款金额无效或余额不足")
    
    def get_balance(self):
        return self.__balance
    
    def __str__(self):
        return f"账户 {self.account_number} - 持有人: {self.owner} - 余额: {self.__balance}"

# 主程序
if __name__ == "__main__":
    print("=== 列表示例 ===")
    list_operations()
    
    print("\n=== 字典示例 ===")
    dict_operations()
    
    print("\n=== 函数装饰器示例 ===")
    print(f"5的阶乘: {calculate_factorial(5)}")
    print(f"斐波那契数列第10项: {fibonacci(10)}")
    
    print("\n=== 文件操作示例 ===")
    file_operations()
    
    print("\n=== 银行账户示例 ===")
    account = BankAccount("1234567890", "李四", 1000.0)
    print(account)
    account.deposit(500)
    account.withdraw(300)
    print(f"当前余额: {account.get_balance()}")
    print(account)
```

## 5. 官方注意事项 / 最佳实践

1. **代码风格**
   - 遵循 PEP 8 代码风格指南
   - 使用 4 个空格缩进
   - 函数和类之间使用两个空行
   - 使用有意义的变量和函数名

2. **性能优化**
   - 优先使用列表推导式而不是 for 循环
   - 使用生成器处理大数据集
   - 合理使用内置函数和标准库

3. **错误处理**
   - 始终处理可能的异常
   - 不要使用过宽的 except 语句
   - 提供有意义的错误信息

4. **安全建议**
   - 不要信任用户输入
   - 使用参数化查询防止 SQL 注入
   - 敏感信息不要硬编码

## 6. 官方文档参考链接

- 官方文档：https://docs.python.org/zh-cn/3/
- 标准库：https://docs.python.org/zh-cn/3/library/index.html
- PEP 8 风格指南：https://peps.python.org/pep-0008/
- Python 教程：https://docs.python.org/zh-cn/3/tutorial/index.html
