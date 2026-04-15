# 检索索引：本文档收录【TypeScript】【核心类型】知识点，内容100%来自TypeScript官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

TypeScript是一种由微软开发的开源编程语言，它是JavaScript的超集，添加了静态类型定义。TypeScript可以编译成纯JavaScript，编译出来的JavaScript可以运行在任何浏览器、Node.js或其他支持JavaScript的环境中。TypeScript旨在帮助开发者构建大型应用程序，并提供更好的开发体验和工具支持。

## 2. 核心知识点（结构化层级）

### 2.1 基础类型
1. 原始类型：string, number, boolean, null, undefined, symbol, bigint
2. 数组类型
3. 元组类型
4. 枚举类型
5. any类型
6. unknown类型
7. void类型
8. never类型
9. object类型

### 2.2 接口（Interfaces）
1. 接口定义
2. 可选属性
3. 只读属性
4. 索引签名
5. 函数类型
6. 继承接口
7. 混合类型

### 2.3 类型别名（Type Aliases）
1. 类型别名定义
2. 与接口的区别
3. 联合类型
4. 交叉类型

### 2.4 函数类型
1. 函数参数类型
2. 函数返回值类型
3. 可选参数
4. 默认参数
5. 剩余参数
6. 函数重载

### 2.5 类
1. 类的定义
2. 成员访问修饰符（public, private, protected）
3. 只读属性
4. 参数属性
5. 继承
6. 抽象类
7. 存取器（getters/setters）

### 2.6 泛型
1. 泛型函数
2. 泛型接口
3. 泛型类
4. 泛型约束
5. 泛型参数默认类型

### 2.7 类型操作
1. 类型断言
2. 类型守卫
3. 类型推断
4. 类型兼容性
5. 条件类型
6. 映射类型
7. 索引类型

### 2.8 模块与命名空间
1. 模块导入导出
2. 命名空间
3. 模块与命名空间的区别

## 3. 官方标准语法 / API

### 基础类型语法
```typescript
// 原始类型
let name: string = 'Alice';
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;
let uniqueId: symbol = Symbol('id');
let bigNumber: bigint = 12345678901234567890n;

// 数组类型
let numbers: number[] = [1, 2, 3, 4, 5];
let fruits: Array<string> = ['apple', 'banana', 'orange'];

// 元组类型
let person: [string, number, boolean] = ['Bob', 25, true];

// 枚举类型
enum Color {
    Red,
    Green,
    Blue
}
let favoriteColor: Color = Color.Green;

// any类型
let anything: any = 'hello';
anything = 42;
anything = true;

// unknown类型
let unknownValue: unknown = 'world';
if (typeof unknownValue === 'string') {
    console.log(unknownValue.toUpperCase());
}

// void类型
function logMessage(message: string): void {
    console.log(message);
}

// never类型
function error(message: string): never {
    throw new Error(message);
}
```

### 接口语法
```typescript
// 基本接口
interface Person {
    name: string;
    age: number;
}

// 可选属性
interface Config {
    title: string;
    subtitle?: string;
}

// 只读属性
interface Point {
    readonly x: number;
    readonly y: number;
}

// 索引签名
interface StringArray {
    [index: number]: string;
}

// 函数类型
interface SearchFunc {
    (source: string, subString: string): boolean;
}

// 继承接口
interface Shape {
    color: string;
}

interface Square extends Shape {
    sideLength: number;
}

// 混合类型
interface Counter {
    (start: number): string;
    interval: number;
    reset(): void;
}
```

### 类型别名语法
```typescript
// 基本类型别名
type Name = string;
type Age = number;

// 联合类型
type StringOrNumber = string | number;
type Status = 'pending' | 'approved' | 'rejected';

// 交叉类型
type Employee = Person & { employeeId: number };

// 对象类型别名
type User = {
    id: number;
    name: string;
    email: string;
};

// 函数类型别名
type GreetFunction = (name: string) => string;
```

### 函数类型语法
```typescript
// 基本函数
function add(a: number, b: number): number {
    return a + b;
}

// 箭头函数
const multiply = (a: number, b: number): number => a * b;

// 可选参数
function greet(name: string, greeting?: string): string {
    if (greeting) {
        return `${greeting}, ${name}!`;
    }
    return `Hello, ${name}!`;
}

// 默认参数
function createUser(name: string, role: string = 'user'): { name: string; role: string } {
    return { name, role };
}

// 剩余参数
function sum(...numbers: number[]): number {
    return numbers.reduce((acc, curr) => acc + curr, 0);
}

// 函数重载
function combine(a: string, b: string): string;
function combine(a: number, b: number): number;
function combine(a: any, b: any): any {
    return a + b;
}
```

### 类语法
```typescript
// 基本类
class Person {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    greet(): string {
        return `Hello, my name is ${this.name}`;
    }
}

// 访问修饰符
class Employee {
    public name: string;
    private salary: number;
    protected department: string;

    constructor(name: string, salary: number, department: string) {
        this.name = name;
        this.salary = salary;
        this.department = department;
    }
}

// 参数属性
class Student {
    constructor(
        public name: string,
        private studentId: number,
        protected grade: string
    ) {}
}

// 继承
class Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    makeSound(): void {
        console.log('Some sound');
    }
}

class Dog extends Animal {
    breed: string;

    constructor(name: string, breed: string) {
        super(name);
        this.breed = breed;
    }

    makeSound(): void {
        console.log('Woof! Woof!');
    }
}

// 抽象类
abstract class Shape {
    abstract getArea(): number;

    getColor(): string {
        return 'blue';
    }
}

class Circle extends Shape {
    radius: number;

    constructor(radius: number) {
        super();
        this.radius = radius;
    }

    getArea(): number {
        return Math.PI * this.radius * this.radius;
    }
}

// 存取器
class Product {
    private _price: number = 0;

    get price(): number {
        return this._price;
    }

    set price(value: number) {
        if (value >= 0) {
            this._price = value;
        } else {
            throw new Error('Price cannot be negative');
        }
    }
}
```

### 泛型语法
```typescript
// 泛型函数
function identity<T>(arg: T): T {
    return arg;
}

// 泛型接口
interface Box<T> {
    value: T;
}

// 泛型类
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }
}

// 泛型约束
interface Lengthwise {
    length: number;
}

function logLength<T extends Lengthwise>(arg: T): void {
    console.log(arg.length);
}

// 泛型参数默认类型
interface ApiResponse<T = any> {
    data: T;
    status: number;
    message: string;
}
```

## 4. 官方示例代码（可运行）

### 基础类型示例
```typescript
// 原始类型
let username: string = 'johndoe';
let userAge: number = 28;
let isLoggedIn: boolean = true;

// 数组
let scores: number[] = [85, 90, 78, 92];
let tags: Array<string> = ['typescript', 'programming'];

// 元组
let coordinates: [number, number] = [40.7128, -74.0060];
let userInfo: [string, number, boolean] = ['alice', 30, true];

// 枚举
enum OrderStatus {
    Pending,
    Shipped,
    Delivered,
    Cancelled
}

let currentStatus: OrderStatus = OrderStatus.Shipped;

console.log('Username:', username);
console.log('Scores:', scores);
console.log('Coordinates:', coordinates);
console.log('Order Status:', OrderStatus[currentStatus]);
```

### 接口示例
```typescript
// 定义用户接口
interface User {
    id: number;
    name: string;
    email: string;
    age?: number;
    readonly createdAt: Date;
}

// 创建用户对象
const user: User = {
    id: 1,
    name: 'Bob Smith',
    email: 'bob@example.com',
    createdAt: new Date()
};

// 定义产品接口
interface Product {
    name: string;
    price: number;
    description?: string;
}

// 定义购物车接口
interface ShoppingCart {
    products: Product[];
    addProduct(product: Product): void;
    getTotal(): number;
}

// 实现购物车
const cart: ShoppingCart = {
    products: [],
    addProduct(product: Product) {
        this.products.push(product);
    },
    getTotal() {
        return this.products.reduce((total, product) => total + product.price, 0);
    }
};

// 添加产品
cart.addProduct({ name: 'Laptop', price: 999 });
cart.addProduct({ name: 'Mouse', price: 25 });

console.log('User:', user);
console.log('Cart Total:', cart.getTotal());
```

### 类与继承示例
```typescript
// 基类
abstract class Vehicle {
    protected brand: string;
    protected year: number;

    constructor(brand: string, year: number) {
        this.brand = brand;
        this.year = year;
    }

    abstract startEngine(): void;

    getInfo(): string {
        return `${this.year} ${this.brand}`;
    }
}

// 汽车类
class Car extends Vehicle {
    private model: string;
    private numberOfDoors: number;

    constructor(brand: string, year: number, model: string, numberOfDoors: number) {
        super(brand, year);
        this.model = model;
        this.numberOfDoors = numberOfDoors;
    }

    startEngine(): void {
        console.log(`${this.brand} ${this.model} engine started. Vroom!`);
    }

    getInfo(): string {
        return `${super.getInfo()} ${this.model} (${this.numberOfDoors} doors)`;
    }
}

// 自行车类
class Bicycle extends Vehicle {
    private hasGears: boolean;

    constructor(brand: string, year: number, hasGears: boolean) {
        super(brand, year);
        this.hasGears = hasGears;
    }

    startEngine(): void {
        console.log('Bicycles don\'t have engines. Start pedaling!');
    }
}

// 使用类
const myCar = new Car('Toyota', 2022, 'Camry', 4);
const myBike = new Bicycle('Giant', 2021, true);

console.log(myCar.getInfo());
myCar.startEngine();

console.log(myBike.getInfo());
myBike.startEngine();
```

### 泛型示例
```typescript
// 泛型栈类
class Stack<T> {
    private items: T[] = [];

    push(item: T): void {
        this.items.push(item);
    }

    pop(): T | undefined {
        return this.items.pop();
    }

    peek(): T | undefined {
        return this.items[this.items.length - 1];
    }

    isEmpty(): boolean {
        return this.items.length === 0;
    }

    size(): number {
        return this.items.length;
    }
}

// 使用数字栈
const numberStack = new Stack<number>();
numberStack.push(1);
numberStack.push(2);
numberStack.push(3);
console.log('Number Stack Top:', numberStack.peek());
console.log('Number Stack Size:', numberStack.size());
console.log('Popped:', numberStack.pop());

// 使用字符串栈
const stringStack = new Stack<string>();
stringStack.push('Hello');
stringStack.push('World');
console.log('String Stack Top:', stringStack.peek());

// 泛型函数
function mergeArrays<T>(arr1: T[], arr2: T[]): T[] {
    return [...arr1, ...arr2];
}

const mergedNumbers = mergeArrays([1, 2, 3], [4, 5, 6]);
const mergedStrings = mergeArrays(['a', 'b'], ['c', 'd']);
console.log('Merged Numbers:', mergedNumbers);
console.log('Merged Strings:', mergedStrings);

// 泛型约束
interface HasId {
    id: number;
}

function findById<T extends HasId>(items: T[], id: number): T | undefined {
    return items.find(item => item.id === id);
}

const users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' }
];

const user = findById(users, 2);
console.log('Found User:', user);
```

### 类型操作示例
```typescript
// 类型守卫
function isString(value: unknown): value is string {
    return typeof value === 'string';
}

function isNumber(value: unknown): value is number {
    return typeof value === 'number';
}

function processValue(value: unknown): void {
    if (isString(value)) {
        console.log('String:', value.toUpperCase());
    } else if (isNumber(value)) {
        console.log('Number:', value * 2);
    } else {
        console.log('Other type');
    }
}

processValue('hello');
processValue(42);

// 条件类型
type IsString<T> = T extends string ? true : false;
type A = IsString<string>; // true
type B = IsString<number>; // false

// 映射类型
type Readonly<T> = {
    readonly [P in keyof T]: T[P];
};

type Partial<T> = {
    [P in keyof T]?: T[P];
};

interface Person {
    name: string;
    age: number;
}

type ReadonlyPerson = Readonly<Person>;
type PartialPerson = Partial<Person>;

// 索引类型
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
    return obj[key];
}

const person = { name: 'Alice', age: 30 };
const name = getProperty(person, 'name');
const age = getProperty(person, 'age');
console.log('Name:', name);
console.log('Age:', age);
```

## 5. 官方注意事项 / 最佳实践

### 类型使用
1. 优先使用接口（interface）定义对象类型，使用类型别名（type）定义联合类型、交叉类型等
2. 避免使用any类型，优先使用unknown类型
3. 使用类型守卫缩小类型范围
4. 善用TypeScript的类型推断，不必为每个变量都显式声明类型

### 类与接口
1. 使用访问修饰符（public, private, protected）控制访问权限
2. 优先使用组合而非继承
3. 使用抽象类定义共享行为
4. 实现接口时确保所有必需的属性和方法都已实现

### 泛型
1. 使用泛型创建可重用的组件
2. 使用泛型约束限制类型参数的范围
3. 为泛型参数提供默认类型以提高易用性

### 最佳实践
1. 启用严格模式（strict: true）
2. 使用tsconfig.json配置TypeScript编译选项
3. 定期运行类型检查
4. 为第三方库安装类型声明文件（@types/*）
5. 使用类型注解提高代码可读性
6. 避免类型断言，除非绝对必要
7. 使用字面量类型和枚举提高类型安全性
8. 遵循TypeScript官方风格指南

## 6. 官方文档参考链接

- TypeScript官方文档：https://www.typescriptlang.org/docs/
- TypeScript手册：https://www.typescriptlang.org/docs/handbook/intro.html
- TypeScript参考：https://www.typescriptlang.org/docs/handbook/reference.html
- TypeScript Playground：https://www.typescriptlang.org/play
