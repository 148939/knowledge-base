# 检索索引：本文档收录【JavaScript】【核心基础】知识点，内容100%来自MDN官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

JavaScript（通常缩写为JS）是一种轻量级、解释型或即时编译型的编程语言，具有头等函数。虽然它以作为Web页面的脚本语言而闻名，但也被用于许多非浏览器环境，如Node.js、Apache CouchDB等。JavaScript是一种基于原型、多范式、单线程、动态语言，支持面向对象、命令式和声明式（如函数式编程）风格。

## 2. 核心知识点（结构化层级）

### 2.1 变量与数据类型
1. 变量声明
   - var（函数作用域）
   - let（块级作用域）
   - const（块级作用域，常量）

2. 数据类型
   - 原始类型：undefined, null, boolean, number, string, symbol, bigint
   - 引用类型：Object（Array, Function, Date, RegExp等）

3. 类型转换
   - 隐式类型转换
   - 显式类型转换

### 2.2 运算符与表达式
1. 算术运算符：+, -, *, /, %, **, ++, --
2. 赋值运算符：=, +=, -=, *=, /=, %=, **=
3. 比较运算符：==, ===, !=, !==, <, >, <=, >=
4. 逻辑运算符：&&, ||, !
5. 条件运算符：condition ? exprIfTrue : exprIfFalse
6. 可选链运算符：?.
7. 空值合并运算符：??

### 2.3 控制流程
1. 条件语句
   - if...else
   - switch

2. 循环语句
   - for
   - while
   - do...while
   - for...in
   - for...of

3. 跳转语句
   - break
   - continue
   - return
   - throw

### 2.4 函数
1. 函数声明
2. 函数表达式
3. 箭头函数
4. 参数
   - 默认参数
   - 剩余参数
   - 参数解构

5. 作用域与闭包
6. this关键字
7. 高阶函数
8. 回调函数

### 2.5 对象与数组
1. 对象字面量
2. 属性访问（点表示法、方括号表示法）
3. 对象方法
4. 数组创建与操作
5. 数组方法（map, filter, reduce, forEach等）
6. 解构赋值
7. 扩展运算符

### 2.6 原型与继承
1. 原型链
2. 构造函数
3. class语法
4. extends继承
5. super关键字

### 2.7 异步编程
1. 回调函数
2. Promise
3. async/await
4. 事件循环

## 3. 官方标准语法 / API

### 变量声明语法
```javascript
// var声明（函数作用域）
var name = 'John';

// let声明（块级作用域）
let age = 25;

// const声明（块级作用域，常量）
const PI = 3.14159;
```

### 函数语法
```javascript
// 函数声明
function greet(name) {
    return 'Hello, ' + name + '!';
}

// 函数表达式
const add = function(a, b) {
    return a + b;
};

// 箭头函数
const multiply = (a, b) => a * b;

// 带默认参数的函数
function greetWithDefault(name = 'Guest') {
    return 'Hello, ' + name + '!';
}

// 剩余参数
function sum(...numbers) {
    return numbers.reduce((acc, curr) => acc + curr, 0);
}
```

### 数组方法语法
```javascript
// map
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(num => num * 2);

// filter
const evens = numbers.filter(num => num % 2 === 0);

// reduce
const sum = numbers.reduce((acc, curr) => acc + curr, 0);

// forEach
numbers.forEach(num => console.log(num));
```

### Promise语法
```javascript
// 创建Promise
const myPromise = new Promise((resolve, reject) => {
    setTimeout(() => {
        resolve('Success!');
    }, 1000);
});

// 使用Promise
myPromise
    .then(result => console.log(result))
    .catch(error => console.error(error));

// async/await
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        return data;
    } catch (error) {
        console.error('Error:', error);
    }
}
```

## 4. 官方示例代码（可运行）

### 变量与数据类型示例
```javascript
// 变量声明
let message = 'Hello, World!';
const count = 10;

// 数据类型
let isActive = true;
let price = 19.99;
let name = 'Alice';
let nothing = null;
let notDefined;
let uniqueId = Symbol('id');
let bigNumber = 12345678901234567890n;

// 数组
let fruits = ['apple', 'banana', 'orange'];

// 对象
let person = {
    name: 'Bob',
    age: 30,
    isStudent: false,
    greet: function() {
        return 'Hi, I\'m ' + this.name;
    }
};

console.log(message);
console.log(person.greet());
```

### 函数示例
```javascript
// 普通函数
function calculateArea(width, height) {
    return width * height;
}

// 箭头函数
const calculatePerimeter = (width, height) => 2 * (width + height);

// 高阶函数
function applyOperation(num, operation) {
    return operation(num);
}

const square = x => x * x;
console.log(applyOperation(5, square)); // 25

// 闭包
function createCounter() {
    let count = 0;
    return {
        increment: () => count++,
        decrement: () => count--,
        getCount: () => count
    };
}

const counter = createCounter();
counter.increment();
counter.increment();
console.log(counter.getCount()); // 2
```

### 数组操作示例
```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// map - 转换数组
const squares = numbers.map(num => num * num);
console.log(squares); // [1, 4, 9, 16, 25, 36, 49, 64, 81, 100]

// filter - 筛选数组
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4, 6, 8, 10]

// reduce - 累积计算
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 55

// find - 查找元素
const firstGreaterThan5 = numbers.find(num => num > 5);
console.log(firstGreaterThan5); // 6

// some - 检查是否存在满足条件的元素
const hasNegative = numbers.some(num => num < 0);
console.log(hasNegative); // false

// every - 检查是否所有元素都满足条件
const allPositive = numbers.every(num => num > 0);
console.log(allPositive); // true
```

### 对象操作示例
```javascript
const person = {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
    address: {
        street: '123 Main St',
        city: 'Anytown'
    }
};

// 属性访问
console.log(person.firstName); // John
console.log(person['lastName']); // Doe

// 可选链
console.log(person.address?.city); // Anytown
console.log(person.contact?.phone); // undefined

// 解构赋值
const { firstName, lastName, age } = person;
console.log(firstName, lastName, age);

const { street, city } = person.address;
console.log(street, city);

// 扩展运算符
const updatedPerson = { ...person, age: 31, email: 'john@example.com' };
console.log(updatedPerson);

// Object.keys, Object.values, Object.entries
console.log(Object.keys(person)); // ['firstName', 'lastName', 'age', 'address']
console.log(Object.values(person)); // ['John', 'Doe', 30, { street: '123 Main St', city: 'Anytown' }]
console.log(Object.entries(person));
```

### 异步编程示例
```javascript
// Promise示例
function fetchUserData(userId) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (userId > 0) {
                resolve({ id: userId, name: 'John Doe', email: 'john@example.com' });
            } else {
                reject(new Error('Invalid user ID'));
            }
        }, 1000);
    });
}

// 使用.then()和.catch()
fetchUserData(1)
    .then(user => {
        console.log('User:', user);
        return fetchUserData(2);
    })
    .then(user => console.log('Next user:', user))
    .catch(error => console.error('Error:', error));

// 使用async/await
async function processUsers() {
    try {
        const user1 = await fetchUserData(1);
        console.log('User 1:', user1);
        
        const user2 = await fetchUserData(2);
        console.log('User 2:', user2);
        
        // 并行请求
        const [user3, user4] = await Promise.all([
            fetchUserData(3),
            fetchUserData(4)
        ]);
        console.log('Users 3 and 4:', user3, user4);
    } catch (error) {
        console.error('Error:', error);
    }
}

processUsers();
```

## 5. 官方注意事项 / 最佳实践

### 变量声明
1. 优先使用const，需要重新赋值时使用let，避免使用var
2. 变量名应具有描述性，遵循驼峰命名法
3. 避免在块级作用域外使用let/const声明的变量

### 类型安全
1. 使用严格相等运算符（===）而非抽象相等运算符（==）
2. 注意隐式类型转换
3. 使用typeof检查原始类型，使用Array.isArray()检查数组

### 函数
1. 优先使用箭头函数作为回调，避免this绑定问题
2. 保持函数简短，每个函数只做一件事
3. 使用默认参数而非在函数内部检查undefined
4. 避免使用arguments对象，使用剩余参数（...）代替

### 数组与对象
1. 使用数组字面量[]而非new Array()
2. 使用对象字面量{}而非new Object()
3. 使用扩展运算符（...）复制数组和对象
4. 使用解构赋值从对象和数组中提取值
5. 优先使用数组方法（map, filter, reduce等）而非for循环

### 异步编程
1. 优先使用async/await而非Promise链
2. 始终处理Promise的reject情况
3. 使用Promise.all()处理并行异步操作
4. 避免回调地狱

### 其他最佳实践
1. 使用严格模式（'use strict'）
2. 避免全局变量污染
3. 使用模板字符串而非字符串拼接
4. 合理使用注释，代码应自解释
5. 遵循一致的代码风格

## 6. 官方文档参考链接

- JavaScript官方文档：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript
- JavaScript参考：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference
- JavaScript指南：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide
- 异步JavaScript：https://developer.mozilla.org/zh-CN/docs/Learn/JavaScript/Asynchronous
