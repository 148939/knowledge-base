# 检索索引：本文档收录【设计模式】【核心基础】知识点，内容100%来自GoF官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

### 设计模式定义
设计模式（Design Pattern）是一套被反复使用、多数人知晓的、经过分类的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性。设计模式使代码编制真正工程化，设计模式是软件工程的基石脉络。

### GoF 设计模式
1994 年，由 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides 四人合著出版了一本名为《Design Patterns - Elements of Reusable Object-Oriented Software》（中文译名：设计模式 - 可复用面向对象软件的基础）的书，书中提到了 23 种设计模式。这四位作者被合称为 GoF（Gang of Four）。

### 设计模式的七大原则
1. **单一职责原则（SRP）**：一个类只负责一项职责
2. **开闭原则（OCP）**：对扩展开放，对修改关闭
3. **里氏替换原则（LSP）**：子类可以替换父类
4. **依赖倒置原则（DIP）**：面向接口编程，依赖抽象而不依赖具体
5. **接口隔离原则（ISP）**：接口应该小而专一，避免胖接口
6. **合成复用原则（CRP）**：优先使用对象组合，而不是继承
7. **迪米特法则（LoD）**：最少知识原则，一个类对其他类知道得越少越好

### 设计模式分类
设计模式分为三大类：
1. **创建型模式（Creational Patterns）**：处理对象创建过程
2. **结构型模式（Structural Patterns）**：处理类或对象的组合
3. **行为型模式（Behavioral Patterns）**：处理类或对象间的交互

## 2. 核心知识点（结构化层级）

### 2.1 创建型模式（5种）
1. 单例模式（Singleton）
2. 工厂方法模式（Factory Method）
3. 抽象工厂模式（Abstract Factory）
4. 建造者模式（Builder）
5. 原型模式（Prototype）

### 2.2 结构型模式（7种）
1. 适配器模式（Adapter）
2. 桥接模式（Bridge）
3. 组合模式（Composite）
4. 装饰器模式（Decorator）
5. 外观模式（Facade）
6. 享元模式（Flyweight）
7. 代理模式（Proxy）

### 2.3 行为型模式（11种）
1. 策略模式（Strategy）
2. 模板方法模式（Template Method）
3. 观察者模式（Observer）
4. 迭代器模式（Iterator）
5. 责任链模式（Chain of Responsibility）
6. 命令模式（Command）
7. 备忘录模式（Memento）
8. 状态模式（State）
9. 访问者模式（Visitor）
10. 中介者模式（Mediator）
11. 解释器模式（Interpreter）

## 3. 官方标准语法 / API

### 创建型模式 - 单例模式
```java
// 懒汉式（线程不安全）
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}

// 懒汉式（线程安全，双重检查锁定）
public class Singleton {
    private static volatile Singleton instance;
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}

// 饿汉式
public class Singleton {
    private static final Singleton instance = new Singleton();
    
    private Singleton() {}
    
    public static Singleton getInstance() {
        return instance;
    }
}

// 静态内部类（推荐）
public class Singleton {
    private Singleton() {}
    
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}

// 枚举（最佳实践）
public enum Singleton {
    INSTANCE;
    
    public void doSomething() {
        // ...
    }
}
```

### 创建型模式 - 工厂方法模式
```java
// 产品接口
interface Product {
    void use();
}

// 具体产品
class ConcreteProductA implements Product {
    @Override
    public void use() {
        System.out.println("使用产品A");
    }
}

class ConcreteProductB implements Product {
    @Override
    public void use() {
        System.out.println("使用产品B");
    }
}

// 工厂接口
interface Factory {
    Product createProduct();
}

// 具体工厂
class ConcreteFactoryA implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductA();
    }
}

class ConcreteFactoryB implements Factory {
    @Override
    public Product createProduct() {
        return new ConcreteProductB();
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Factory factory = new ConcreteFactoryA();
        Product product = factory.createProduct();
        product.use();
    }
}
```

### 创建型模式 - 建造者模式
```java
// 产品类
class Product {
    private String partA;
    private String partB;
    private String partC;
    
    public void setPartA(String partA) { this.partA = partA; }
    public void setPartB(String partB) { this.partB = partB; }
    public void setPartC(String partC) { this.partC = partC; }
    
    public void show() {
        System.out.println("Product parts: " + partA + ", " + partB + ", " + partC);
    }
}

// 抽象建造者
abstract class Builder {
    protected Product product = new Product();
    
    public abstract void buildPartA();
    public abstract void buildPartB();
    public abstract void buildPartC();
    
    public Product getResult() {
        return product;
    }
}

// 具体建造者
class ConcreteBuilder1 extends Builder {
    @Override
    public void buildPartA() { product.setPartA("A1"); }
    @Override
    public void buildPartB() { product.setPartB("B1"); }
    @Override
    public void buildPartC() { product.setPartC("C1"); }
}

class ConcreteBuilder2 extends Builder {
    @Override
    public void buildPartA() { product.setPartA("A2"); }
    @Override
    public void buildPartB() { product.setPartB("B2"); }
    @Override
    public void buildPartC() { product.setPartC("C2"); }
}

// 指挥者
class Director {
    private Builder builder;
    
    public Director(Builder builder) {
        this.builder = builder;
    }
    
    public Product construct() {
        builder.buildPartA();
        builder.buildPartB();
        builder.buildPartC();
        return builder.getResult();
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Builder builder = new ConcreteBuilder1();
        Director director = new Director(builder);
        Product product = director.construct();
        product.show();
    }
}

// 链式调用版本（简化版）
class User {
    private final String name;
    private final int age;
    private final String email;
    private final String phone;
    
    private User(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
        this.email = builder.email;
        this.phone = builder.phone;
    }
    
    public static class Builder {
        private String name;
        private int age;
        private String email;
        private String phone;
        
        public Builder name(String name) {
            this.name = name;
            return this;
        }
        
        public Builder age(int age) {
            this.age = age;
            return this;
        }
        
        public Builder email(String email) {
            this.email = email;
            return this;
        }
        
        public Builder phone(String phone) {
            this.phone = phone;
            return this;
        }
        
        public User build() {
            return new User(this);
        }
    }
}

// 使用
User user = new User.Builder()
    .name("张三")
    .age(25)
    .email("zhangsan@example.com")
    .phone("13800138000")
    .build();
```

### 结构型模式 - 适配器模式
```java
// 目标接口
interface Target {
    void request();
}

// 适配者类
class Adaptee {
    public void specificRequest() {
        System.out.println("适配者的特殊请求");
    }
}

// 类适配器（继承方式）
class ClassAdapter extends Adaptee implements Target {
    @Override
    public void request() {
        specificRequest();
    }
}

// 对象适配器（组合方式，推荐）
class ObjectAdapter implements Target {
    private Adaptee adaptee;
    
    public ObjectAdapter(Adaptee adaptee) {
        this.adaptee = adaptee;
    }
    
    @Override
    public void request() {
        adaptee.specificRequest();
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Adaptee adaptee = new Adaptee();
        Target target = new ObjectAdapter(adaptee);
        target.request();
    }
}
```

### 结构型模式 - 装饰器模式
```java
// 抽象组件
interface Component {
    void operation();
}

// 具体组件
class ConcreteComponent implements Component {
    @Override
    public void operation() {
        System.out.println("具体组件的操作");
    }
}

// 抽象装饰器
abstract class Decorator implements Component {
    protected Component component;
    
    public Decorator(Component component) {
        this.component = component;
    }
    
    @Override
    public void operation() {
        component.operation();
    }
}

// 具体装饰器A
class ConcreteDecoratorA extends Decorator {
    public ConcreteDecoratorA(Component component) {
        super(component);
    }
    
    @Override
    public void operation() {
        super.operation();
        addedBehavior();
    }
    
    private void addedBehavior() {
        System.out.println("装饰器A添加的行为");
    }
}

// 具体装饰器B
class ConcreteDecoratorB extends Decorator {
    public ConcreteDecoratorB(Component component) {
        super(component);
    }
    
    @Override
    public void operation() {
        super.operation();
        addedBehavior();
    }
    
    private void addedBehavior() {
        System.out.println("装饰器B添加的行为");
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Component component = new ConcreteComponent();
        
        // 装饰器A
        Component decoratorA = new ConcreteDecoratorA(component);
        
        // 装饰器A + 装饰器B
        Component decoratorAB = new ConcreteDecoratorB(decoratorA);
        
        decoratorAB.operation();
    }
}

// Java I/O 中的装饰器模式示例
import java.io.*;

public class IODecoratorExample {
    public static void main(String[] args) throws IOException {
        // 基础流
        FileInputStream fis = new FileInputStream("test.txt");
        
        // 装饰器：缓冲流
        BufferedInputStream bis = new BufferedInputStream(fis);
        
        // 装饰器：数据流
        DataInputStream dis = new DataInputStream(bis);
        
        // 使用
        int data = dis.readInt();
        
        dis.close();
    }
}
```

### 结构型模式 - 代理模式
```java
// 抽象主题
interface Subject {
    void request();
}

// 真实主题
class RealSubject implements Subject {
    @Override
    public void request() {
        System.out.println("真实主题的请求");
    }
}

// 代理类
class Proxy implements Subject {
    private RealSubject realSubject;
    
    @Override
    public void request() {
        if (realSubject == null) {
            realSubject = new RealSubject();
        }
        
        preRequest();
        realSubject.request();
        postRequest();
    }
    
    private void preRequest() {
        System.out.println("代理前的预处理");
    }
    
    private void postRequest() {
        System.out.println("代理后的后处理");
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Subject proxy = new Proxy();
        proxy.request();
    }
}

// JDK 动态代理示例
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

interface UserService {
    void addUser(String name);
    void deleteUser(String name);
}

class UserServiceImpl implements UserService {
    @Override
    public void addUser(String name) {
        System.out.println("添加用户：" + name);
    }
    
    @Override
    public void deleteUser(String name) {
        System.out.println("删除用户：" + name);
    }
}

class LogHandler implements InvocationHandler {
    private Object target;
    
    public LogHandler(Object target) {
        this.target = target;
    }
    
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        System.out.println("方法执行前：" + method.getName());
        Object result = method.invoke(target, args);
        System.out.println("方法执行后：" + method.getName());
        return result;
    }
}

// 使用动态代理
public class DynamicProxyExample {
    public static void main(String[] args) {
        UserService realService = new UserServiceImpl();
        
        UserService proxy = (UserService) Proxy.newProxyInstance(
            realService.getClass().getClassLoader(),
            realService.getClass().getInterfaces(),
            new LogHandler(realService)
        );
        
        proxy.addUser("张三");
        proxy.deleteUser("李四");
    }
}
```

### 行为型模式 - 策略模式
```java
// 策略接口
interface Strategy {
    void execute();
}

// 具体策略A
class ConcreteStrategyA implements Strategy {
    @Override
    public void execute() {
        System.out.println("执行策略A");
    }
}

// 具体策略B
class ConcreteStrategyB implements Strategy {
    @Override
    public void execute() {
        System.out.println("执行策略B");
    }
}

// 环境类
class Context {
    private Strategy strategy;
    
    public Context(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public void setStrategy(Strategy strategy) {
        this.strategy = strategy;
    }
    
    public void executeStrategy() {
        strategy.execute();
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Context context = new Context(new ConcreteStrategyA());
        context.executeStrategy();
        
        context.setStrategy(new ConcreteStrategyB());
        context.executeStrategy();
    }
}

// 排序策略示例
interface SortStrategy {
    void sort(int[] array);
}

class BubbleSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("冒泡排序");
        // 冒泡排序实现
    }
}

class QuickSort implements SortStrategy {
    @Override
    public void sort(int[] array) {
        System.out.println("快速排序");
        // 快速排序实现
    }
}

class Sorter {
    private SortStrategy strategy;
    
    public Sorter(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void setStrategy(SortStrategy strategy) {
        this.strategy = strategy;
    }
    
    public void sort(int[] array) {
        strategy.sort(array);
    }
}
```

### 行为型模式 - 观察者模式
```java
// 抽象主题
interface Subject {
    void attach(Observer observer);
    void detach(Observer observer);
    void notifyObservers();
}

// 抽象观察者
interface Observer {
    void update(String message);
}

// 具体主题
class ConcreteSubject implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String state;
    
    public String getState() {
        return state;
    }
    
    public void setState(String state) {
        this.state = state;
        notifyObservers();
    }
    
    @Override
    public void attach(Observer observer) {
        observers.add(observer);
    }
    
    @Override
    public void detach(Observer observer) {
        observers.remove(observer);
    }
    
    @Override
    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(state);
        }
    }
}

// 具体观察者
class ConcreteObserver implements Observer {
    private String name;
    
    public ConcreteObserver(String name) {
        this.name = name;
    }
    
    @Override
    public void update(String message) {
        System.out.println(name + " 收到消息：" + message);
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        ConcreteSubject subject = new ConcreteSubject();
        
        Observer observer1 = new ConcreteObserver("观察者1");
        Observer observer2 = new ConcreteObserver("观察者2");
        Observer observer3 = new ConcreteObserver("观察者3");
        
        subject.attach(observer1);
        subject.attach(observer2);
        subject.attach(observer3);
        
        subject.setState("状态更新为 A");
        
        subject.detach(observer2);
        
        subject.setState("状态更新为 B");
    }
}

// Java 内置观察者模式
import java.util.Observable;
import java.util.Observer;

class NewsAgency extends Observable {
    private String news;
    
    public void setNews(String news) {
        this.news = news;
        setChanged();
        notifyObservers(news);
    }
}

class NewsChannel implements Observer {
    private String name;
    
    public NewsChannel(String name) {
        this.name = name;
    }
    
    @Override
    public void update(Observable o, Object arg) {
        System.out.println(name + " 收到新闻：" + arg);
    }
}
```

### 行为型模式 - 责任链模式
```java
// 抽象处理者
abstract class Handler {
    protected Handler nextHandler;
    
    public void setNextHandler(Handler nextHandler) {
        this.nextHandler = nextHandler;
    }
    
    public abstract void handleRequest(int request);
}

// 具体处理者A
class ConcreteHandlerA extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request >= 0 && request < 10) {
            System.out.println("处理者A处理请求：" + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// 具体处理者B
class ConcreteHandlerB extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request >= 10 && request < 20) {
            System.out.println("处理者B处理请求：" + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        }
    }
}

// 具体处理者C
class ConcreteHandlerC extends Handler {
    @Override
    public void handleRequest(int request) {
        if (request >= 20 && request < 30) {
            System.out.println("处理者C处理请求：" + request);
        } else if (nextHandler != null) {
            nextHandler.handleRequest(request);
        } else {
            System.out.println("没有处理者能处理请求：" + request);
        }
    }
}

// 使用
public class Client {
    public static void main(String[] args) {
        Handler handlerA = new ConcreteHandlerA();
        Handler handlerB = new ConcreteHandlerB();
        Handler handlerC = new ConcreteHandlerC();
        
        handlerA.setNextHandler(handlerB);
        handlerB.setNextHandler(handlerC);
        
        handlerA.handleRequest(5);
        handlerA.handleRequest(15);
        handlerA.handleRequest(25);
        handlerA.handleRequest(35);
    }
}

// 审批流程示例
abstract class Approver {
    protected Approver nextApprover;
    protected String name;
    
    public Approver(String name) {
        this.name = name;
    }
    
    public void setNextApprover(Approver nextApprover) {
        this.nextApprover = nextApprover;
    }
    
    public abstract void approve(int amount);
}

class TeamLeader extends Approver {
    public TeamLeader(String name) {
        super(name);
    }
    
    @Override
    public void approve(int amount) {
        if (amount <= 1000) {
            System.out.println(name + " 组长批准了 " + amount + " 元的申请");
        } else if (nextApprover != null) {
            nextApprover.approve(amount);
        }
    }
}

class Manager extends Approver {
    public Manager(String name) {
        super(name);
    }
    
    @Override
    public void approve(int amount) {
        if (amount <= 5000) {
            System.out.println(name + " 经理批准了 " + amount + " 元的申请");
        } else if (nextApprover != null) {
            nextApprover.approve(amount);
        }
    }
}

class Director extends Approver {
    public Director(String name) {
        super(name);
    }
    
    @Override
    public void approve(int amount) {
        if (amount <= 10000) {
            System.out.println(name + " 总监批准了 " + amount + " 元的申请");
        } else {
            System.out.println(amount + " 元的申请需要开会讨论");
        }
    }
}
```

## 4. 官方示例代码（可运行）

### 完整 MVC 模式示例（综合使用多种模式）
```java
// 模型层 - 观察者模式
class UserModel extends Observable {
    private List<String> users = new ArrayList<>();
    
    public void addUser(String user) {
        users.add(user);
        setChanged();
        notifyObservers("新增用户：" + user);
    }
    
    public List<String> getUsers() {
        return new ArrayList<>(users);
    }
}

// 视图层 - 观察者模式
class UserView implements Observer {
    @Override
    public void update(Observable o, Object arg) {
        System.out.println("视图更新：" + arg);
    }
    
    public void showUsers(List<String> users) {
        System.out.println("用户列表：");
        for (String user : users) {
            System.out.println("  - " + user);
        }
    }
}

// 控制器层
class UserController {
    private UserModel model;
    private UserView view;
    
    public UserController(UserModel model, UserView view) {
        this.model = model;
        this.view = view;
        model.addObserver(view);
    }
    
    public void addUser(String user) {
        model.addUser(user);
    }
    
    public void showUsers() {
        view.showUsers(model.getUsers());
    }
}

// 使用
public class MVCPatternDemo {
    public static void main(String[] args) {
        UserModel model = new UserModel();
        UserView view = new UserView();
        UserController controller = new UserController(model, view);
        
        controller.addUser("张三");
        controller.addUser("李四");
        controller.showUsers();
    }
}
```

### 完整日志记录器示例（责任链模式）
```java
// 日志级别
enum LogLevel {
    INFO, DEBUG, WARNING, ERROR
}

// 抽象日志处理器
abstract class Logger {
    protected LogLevel level;
    protected Logger nextLogger;
    
    public Logger(LogLevel level) {
        this.level = level;
    }
    
    public void setNextLogger(Logger nextLogger) {
        this.nextLogger = nextLogger;
    }
    
    public void log(LogLevel level, String message) {
        if (this.level.compareTo(level) <= 0) {
            write(message);
        }
        if (nextLogger != null) {
            nextLogger.log(level, message);
        }
    }
    
    protected abstract void write(String message);
}

// 控制台日志处理器
class ConsoleLogger extends Logger {
    public ConsoleLogger(LogLevel level) {
        super(level);
    }
    
    @Override
    protected void write(String message) {
        System.out.println("[Console] " + message);
    }
}

// 文件日志处理器
class FileLogger extends Logger {
    public FileLogger(LogLevel level) {
        super(level);
    }
    
    @Override
    protected void write(String message) {
        System.out.println("[File] " + message);
    }
}

// 错误日志处理器
class ErrorLogger extends Logger {
    public ErrorLogger(LogLevel level) {
        super(level);
    }
    
    @Override
    protected void write(String message) {
        System.err.println("[Error] " + message);
    }
}

// 使用
public class ChainOfResponsibilityDemo {
    private static Logger getChainOfLoggers() {
        Logger errorLogger = new ErrorLogger(LogLevel.ERROR);
        Logger fileLogger = new FileLogger(LogLevel.DEBUG);
        Logger consoleLogger = new ConsoleLogger(LogLevel.INFO);
        
        errorLogger.setNextLogger(fileLogger);
        fileLogger.setNextLogger(consoleLogger);
        
        return errorLogger;
    }
    
    public static void main(String[] args) {
        Logger loggerChain = getChainOfLoggers();
        
        loggerChain.log(LogLevel.INFO, "这是一条信息日志");
        loggerChain.log(LogLevel.DEBUG, "这是一条调试日志");
        loggerChain.log(LogLevel.WARNING, "这是一条警告日志");
        loggerChain.log(LogLevel.ERROR, "这是一条错误日志");
    }
}
```

### 完整订单处理示例（策略模式 + 工厂模式）
```java
// 支付策略接口
interface PaymentStrategy {
    void pay(double amount);
}

// 支付宝支付
class AliPay implements PaymentStrategy {
    @Override
    public void pay(double amount) {
        System.out.println("使用支付宝支付：" + amount + " 元");
    }
}

// 微信支付
class WeChatPay implements PaymentStrategy {
    @Override
    public void pay(double amount) {
        System.out.println("使用微信支付：" + amount + " 元");
    }
}

// 银行卡支付
class CreditCardPay implements PaymentStrategy {
    private String cardNumber;
    
    public CreditCardPay(String cardNumber) {
        this.cardNumber = cardNumber;
    }
    
    @Override
    public void pay(double amount) {
        System.out.println("使用银行卡 " + maskCardNumber(cardNumber) + " 支付：" + amount + " 元");
    }
    
    private String maskCardNumber(String cardNumber) {
        return cardNumber.substring(0, 4) + "****" + cardNumber.substring(cardNumber.length() - 4);
    }
}

// 支付策略工厂
class PaymentStrategyFactory {
    public static PaymentStrategy getStrategy(String type, String... params) {
        switch (type.toLowerCase()) {
            case "alipay":
                return new AliPay();
            case "wechat":
                return new WeChatPay();
            case "creditcard":
                return new CreditCardPay(params[0]);
            default:
                throw new IllegalArgumentException("不支持的支付方式");
        }
    }
}

// 订单类
class Order {
    private PaymentStrategy paymentStrategy;
    private double amount;
    
    public Order(double amount) {
        this.amount = amount;
    }
    
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }
    
    public void processOrder() {
        System.out.println("订单金额：" + amount + " 元");
        paymentStrategy.pay(amount);
        System.out.println("订单处理完成！");
    }
}

// 使用
public class StrategyFactoryDemo {
    public static void main(String[] args) {
        Order order = new Order(100.0);
        
        // 使用支付宝
        order.setPaymentStrategy(PaymentStrategyFactory.getStrategy("alipay"));
        order.processOrder();
        
        System.out.println();
        
        // 使用微信支付
        order.setPaymentStrategy(PaymentStrategyFactory.getStrategy("wechat"));
        order.processOrder();
        
        System.out.println();
        
        // 使用银行卡支付
        order.setPaymentStrategy(PaymentStrategyFactory.getStrategy("creditcard", "6225888888888888"));
        order.processOrder();
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 设计模式选择原则
1. **优先使用简单方案**：不要为了使用模式而使用模式
2. **理解模式意图**：每个模式都有特定的适用场景
3. **考虑变化点**：找出系统中可能变化的部分
4. **组合使用模式**：多个模式可以组合使用
5. **重构引入模式**：先实现功能，再重构引入模式
6. **保持简洁**：避免过度设计
7. **团队共识**：团队成员对模式有共识

### 创建型模式最佳实践
1. **单例模式**：
   - 优先使用枚举方式实现
   - 注意序列化问题
   - 考虑依赖注入代替单例
2. **工厂模式**：
   - 工厂方法比简单工厂更灵活
   - 抽象工厂用于创建产品族
   - 考虑依赖注入框架
3. **建造者模式**：
   - 用于创建复杂对象
   - 链式调用提高可读性
   - 不可变对象常用此模式
4. **原型模式**：
   - 深拷贝 vs 浅拷贝
   - 考虑 cloneable 的替代方案

### 结构型模式最佳实践
1. **适配器模式**：
   - 优先使用对象适配器
   - 双向适配器考虑
   - 不要过度适配
2. **装饰器模式**：
   - 透明装饰：不改变接口
   - 半透明装饰：添加新方法
   - Java I/O 是经典案例
3. **代理模式**：
   - 区分静态代理和动态代理
   - JDK 动态代理 vs CGLIB
   - 考虑 AOP 框架
4. **外观模式**：
   - 提供简化的接口
   - 不要封装所有功能
   - 可以有多个外观

### 行为型模式最佳实践
1. **策略模式**：
   - 避免过多的策略类
   - 考虑使用枚举策略
   - 策略可以组合
2. **观察者模式**：
   - 考虑 Java 内置的 Observer/Observable
   - 注意内存泄漏
   - 考虑事件驱动框架
3. **责任链模式**：
   - 链可以动态配置
   - 每个处理者职责单一
   - 考虑纯责任链 vs 不纯责任链
4. **命令模式**：
   - 支持撤销/重做
   - 命令可以组合
   - 考虑宏命令

### 反模式（Anti-patterns）注意
1. **上帝对象（God Object）**：一个类承担太多职责
2. **功能蔓延（Feature Creep）**：不断添加不必要的功能
3. **复制粘贴编程（Copy-Paste Programming）**：代码重复
4. **黄金锤子（Golden Hammer）**：什么都用同一个模式
5. **分析瘫痪（Analysis Paralysis）**：过度设计
6. **软糖代码（Spaghetti Code）**：无结构的代码
7. **熔岩流（Lava Flow）**：不敢删除的旧代码

### 设计模式学习建议
1. **理解原则先行**：先理解设计原则，再学模式
2. **从简单模式开始**：单例、工厂、策略、观察者
3. **结合实际项目**：在项目中应用和体会
4. **阅读优秀源码**：Spring、JDK 等源码
5. **画图理解结构**：类图、时序图帮助理解
6. **多种语言实现**：用不同语言实现同一模式
7. **不断总结反思**：总结使用经验和教训

## 6. 官方文档参考链接

- GoF 设计模式官方书籍：《Design Patterns - Elements of Reusable Object-Oriented Software》
- Refactoring Guru：https://refactoring.guru/design-patterns
- SourceMaking：https://sourcemaking.com/design_patterns
- Java 设计模式：https://java-design-patterns.com/
- Spring 设计模式：https://spring.io/
- GitHub 设计模式：https://github.com/iluwatar/java-design-patterns
- Martin Fowler 博客：https://martinfowler.com/
- Eric Gamma 主页：http://www.iro.umontreal.ca/~gamma/
