# 检索索引：本文档收录【Java】【面向对象编程】知识点，内容100%来自Java官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

面向对象编程（Object-Oriented Programming, OOP）是 Java 的核心编程范式。它将数据（属性）和操作数据的方法（行为）封装在对象中。Java 的面向对象特性包括：封装（Encapsulation）、继承（Inheritance）、多态（Polymorphism）和抽象（Abstraction）。这些特性使代码更模块化、可复用、可扩展和易于维护。

## 2. 核心知识点（结构化层级）

### 2.1 类与对象
1. 类的定义与结构
2. 对象的创建与使用
3. 构造方法
4. this 关键字
5. 静态成员与实例成员

### 2.2 封装
1. 访问修饰符（public、protected、private、默认）
2. Getter 和 Setter 方法
3. 数据隐藏原则
4. 不可变对象

### 2.3 继承
1. extends 关键字
2. 方法重写（Override）
3. super 关键字
4. 继承层次结构
5. Object 类
6. final 关键字与继承

### 2.4 多态
1. 方法重写（运行时多态）
2. 方法重载（编译时多态）
3. 向上转型与向下转型
4. instanceof 关键字
5. 抽象类与接口

### 2.5 抽象类与接口
1. 抽象类（abstract class）
2. 接口（interface）
3. 抽象类 vs 接口
4. 接口默认方法（Java 8+）
5. 接口静态方法
6. 函数式接口

### 2.6 内部类
1. 成员内部类
2. 静态内部类
3. 局部内部类
4. 匿名内部类

### 2.7 枚举类
1. 枚举定义
2. 枚举方法
3. 枚举构造函数
4. 枚举与 switch

## 3. 官方标准语法 / API

### 类与对象
```java
// 类的定义
public class Person {
    // 成员变量（实例变量）
    private String name;
    private int age;
    
    // 静态变量
    private static int personCount = 0;
    
    // 无参构造方法
    public Person() {
        this.name = "Unknown";
        this.age = 0;
        personCount++;
    }
    
    // 有参构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
        personCount++;
    }
    
    // Getter 方法
    public String getName() {
        return name;
    }
    
    // Setter 方法
    public void setName(String name) {
        this.name = name;
    }
    
    public int getAge() {
        return age;
    }
    
    public void setAge(int age) {
        if (age > 0 && age < 150) {
            this.age = age;
        }
    }
    
    // 实例方法
    public void introduce() {
        System.out.println("My name is " + name + ", I'm " + age + " years old.");
    }
    
    // 静态方法
    public static int getPersonCount() {
        return personCount;
    }
    
    // toString 方法重写
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

// 使用类和对象
public class Main {
    public static void main(String[] args) {
        // 创建对象
        Person person1 = new Person();
        Person person2 = new Person("Alice", 25);
        
        // 调用方法
        person1.introduce();
        person2.introduce();
        
        // 使用 Setter
        person1.setName("Bob");
        person1.setAge(30);
        person1.introduce();
        
        // 调用静态方法
        System.out.println("Total persons: " + Person.getPersonCount());
        
        // toString
        System.out.println(person2);
    }
}
```

### 封装与访问修饰符
```java
public class EncapsulationExample {
    // 不同访问修饰符的成员变量
    public String publicField = "Public";
    protected String protectedField = "Protected";
    String defaultField = "Default"; // 包访问权限
    private String privateField = "Private";
    
    // private 字段通过 getter/setter 访问
    private String secret;
    
    public String getSecret() {
        return secret;
    }
    
    public void setSecret(String secret) {
        this.secret = secret;
    }
    
    // 不可变对象示例
    public final class ImmutablePerson {
        private final String name;
        private final int age;
        
        public ImmutablePerson(String name, int age) {
            this.name = name;
            this.age = age;
        }
        
        // 只有 getter，没有 setter
        public String getName() {
            return name;
        }
        
        public int getAge() {
            return age;
        }
    }
}

// 同一包中的类
class SamePackageClass {
    public void accessFields() {
        EncapsulationExample obj = new EncapsulationExample();
        System.out.println(obj.publicField);      // 可访问
        System.out.println(obj.protectedField);   // 可访问
        System.out.println(obj.defaultField);     // 可访问
        // System.out.println(obj.privateField);  // 不可访问
    }
}
```

### 继承
```java
// 父类（超类）
public class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " is eating.");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping.");
    }
    
    public void makeSound() {
        System.out.println("Some generic sound...");
    }
}

// 子类（派生类）
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        // 调用父类构造方法
        super(name, age);
        this.breed = breed;
    }
    
    // 方法重写
    @Override
    public void makeSound() {
        System.out.println(name + " says: Woof! Woof!");
    }
    
    // 子类特有方法
    public void fetch() {
        System.out.println(name + " is fetching the ball!");
    }
    
    @Override
    public String toString() {
        return "Dog{name='" + name + "', age=" + age + ", breed='" + breed + "'}";
    }
}

public class Cat extends Animal {
    public Cat(String name, int age) {
        super(name, age);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " says: Meow! Meow!");
    }
    
    public void scratch() {
        System.out.println(name + " is scratching!");
    }
}

// 使用继承
public class InheritanceTest {
    public static void main(String[] args) {
        Dog dog = new Dog("Buddy", 3, "Golden Retriever");
        Cat cat = new Cat("Whiskers", 2);
        
        // 继承的方法
        dog.eat();
        dog.sleep();
        
        // 重写的方法
        dog.makeSound();
        cat.makeSound();
        
        // 子类特有方法
        dog.fetch();
        cat.scratch();
        
        System.out.println(dog);
    }
}
```

### 多态
```java
// 多态示例
public class PolymorphismTest {
    public static void main(String[] args) {
        // 向上转型（自动类型转换）
        Animal animal1 = new Dog("Buddy", 3, "Golden Retriever");
        Animal animal2 = new Cat("Whiskers", 2);
        
        // 运行时多态 - 调用的是实际对象类型的方法
        animal1.makeSound();  // 输出: Buddy says: Woof! Woof!
        animal2.makeSound();  // 输出: Whiskers says: Meow! Meow!
        
        // 方法重载 - 编译时多态
        Calculator calc = new Calculator();
        System.out.println(calc.add(5, 3));           // 8
        System.out.println(calc.add(5.5, 3.3));       // 8.8
        System.out.println(calc.add(1, 2, 3));        // 6
        
        // 向下转型（需要强制类型转换）
        if (animal1 instanceof Dog) {
            Dog dog = (Dog) animal1;
            dog.fetch();  // 现在可以调用 Dog 特有方法
        }
        
        // 使用多态数组
        Animal[] animals = {
            new Dog("Max", 5, "Labrador"),
            new Cat("Luna", 1),
            new Dog("Charlie", 2, "Beagle")
        };
        
        for (Animal animal : animals) {
            animal.makeSound();
        }
    }
}

// 方法重载示例
class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
    
    public double add(double a, double b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

### 抽象类与接口
```java
// 抽象类
abstract class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    // 抽象方法 - 没有实现
    public abstract double getArea();
    public abstract double getPerimeter();
    
    // 具体方法
    public String getColor() {
        return color;
    }
    
    public void setColor(String color) {
        this.color = color;
    }
}

// 继承抽象类
class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double getArea() {
        return width * height;
    }
    
    @Override
    public double getPerimeter() {
        return 2 * (width + height);
    }
}

// 接口定义（Java 8 之前）
interface Flyable {
    void fly();  // 隐式 public abstract
}

interface Swimmable {
    void swim();
}

// Java 8+ 接口 - 可以有默认方法和静态方法
interface Drawable {
    void draw();
    
    // 默认方法
    default void drawWithColor(String color) {
        System.out.println("Drawing with color: " + color);
    }
    
    // 静态方法
    static void printInfo() {
        System.out.println("This is a Drawable interface");
    }
}

// 函数式接口（只有一个抽象方法）
@FunctionalInterface
interface Calculable {
    double calculate(double a, double b);
}

// 实现多个接口
class Duck implements Flyable, Swimmable, Drawable {
    private String name;
    
    public Duck(String name) {
        this.name = name;
    }
    
    @Override
    public void fly() {
        System.out.println(name + " is flying!");
    }
    
    @Override
    public void swim() {
        System.out.println(name + " is swimming!");
    }
    
    @Override
    public void draw() {
        System.out.println("Drawing a duck: " + name);
    }
}

// 使用抽象类和接口
public class AbstractInterfaceTest {
    public static void main(String[] args) {
        // 抽象类的使用
        Shape circle = new Circle("Red", 5.0);
        Shape rectangle = new Rectangle("Blue", 4.0, 6.0);
        
        System.out.println("Circle area: " + circle.getArea());
        System.out.println("Circle perimeter: " + circle.getPerimeter());
        System.out.println("Rectangle area: " + rectangle.getArea());
        System.out.println("Rectangle perimeter: " + rectangle.getPerimeter());
        
        // 接口的使用
        Duck duck = new Duck("Donald");
        duck.fly();
        duck.swim();
        duck.draw();
        duck.drawWithColor("Yellow");
        
        Drawable.printInfo();
        
        // 函数式接口与 Lambda 表达式
        Calculable add = (a, b) -> a + b;
        Calculable multiply = (a, b) -> a * b;
        
        System.out.println("Add: " + add.calculate(5, 3));
        System.out.println("Multiply: " + multiply.calculate(5, 3));
    }
}
```

### 内部类
```java
public class OuterClass {
    private String outerField = "Outer Field";
    private static String staticOuterField = "Static Outer Field";
    
    // 1. 成员内部类
    public class MemberInnerClass {
        private String innerField = "Inner Field";
        
        public void display() {
            System.out.println("Outer field: " + outerField);
            System.out.println("Inner field: " + innerField);
        }
    }
    
    // 2. 静态内部类
    public static class StaticInnerClass {
        private String staticInnerField = "Static Inner Field";
        
        public void display() {
            System.out.println("Static outer field: " + staticOuterField);
            System.out.println("Static inner field: " + staticInnerField);
        }
    }
    
    // 3. 局部内部类（在方法中）
    public void methodWithInnerClass() {
        final String localVar = "Local Variable";
        
        class LocalInnerClass {
            public void display() {
                System.out.println("Outer field: " + outerField);
                System.out.println("Local variable: " + localVar);
            }
        }
        
        LocalInnerClass local = new LocalInnerClass();
        local.display();
    }
    
    // 4. 匿名内部类
    public void useAnonymousClass() {
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println("Anonymous inner class running");
            }
        };
        
        Thread thread = new Thread(runnable);
        thread.start();
    }
}

// 使用内部类
public class InnerClassTest {
    public static void main(String[] args) {
        // 成员内部类
        OuterClass outer = new OuterClass();
        OuterClass.MemberInnerClass memberInner = outer.new MemberInnerClass();
        memberInner.display();
        
        // 静态内部类
        OuterClass.StaticInnerClass staticInner = new OuterClass.StaticInnerClass();
        staticInner.display();
        
        // 局部内部类
        outer.methodWithInnerClass();
        
        // 匿名内部类
        outer.useAnonymousClass();
    }
}
```

### 枚举类
```java
// 基本枚举
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

// 带构造函数和方法的枚举
enum Season {
    SPRING("March", "May", "Warm"),
    SUMMER("June", "August", "Hot"),
    AUTUMN("September", "November", "Cool"),
    WINTER("December", "February", "Cold");
    
    private final String startMonth;
    private final String endMonth;
    private final String description;
    
    // 枚举构造函数（隐式 private）
    Season(String startMonth, String endMonth, String description) {
        this.startMonth = startMonth;
        this.endMonth = endMonth;
        this.description = description;
    }
    
    public String getStartMonth() {
        return startMonth;
    }
    
    public String getEndMonth() {
        return endMonth;
    }
    
    public String getDescription() {
        return description;
    }
    
    @Override
    public String toString() {
        return name() + " (" + startMonth + "-" + endMonth + "): " + description;
    }
}

// 使用枚举
public class EnumTest {
    public static void main(String[] args) {
        // 基本枚举使用
        Day today = Day.MONDAY;
        System.out.println("Today is: " + today);
        
        // 枚举与 switch
        switch (today) {
            case MONDAY:
            case TUESDAY:
            case WEDNESDAY:
            case THURSDAY:
            case FRIDAY:
                System.out.println("It's a weekday!");
                break;
            case SATURDAY:
            case SUNDAY:
                System.out.println("It's the weekend!");
                break;
        }
        
        // 遍历枚举
        System.out.println("\nAll days:");
        for (Day day : Day.values()) {
            System.out.println(day.ordinal() + ": " + day);
        }
        
        // valueOf 方法
        Day friday = Day.valueOf("FRIDAY");
        System.out.println("\nFriday: " + friday);
        
        // 带方法的枚举
        System.out.println("\nSeasons:");
        for (Season season : Season.values()) {
            System.out.println(season);
        }
        
        Season summer = Season.SUMMER;
        System.out.println("\nSummer description: " + summer.getDescription());
    }
}
```

## 4. 官方示例代码（可运行）

### 完整的员工管理系统
```java
import java.util.ArrayList;
import java.util.List;

// 抽象类 - 员工基类
abstract class Employee {
    protected int id;
    protected String name;
    protected double baseSalary;
    protected static int employeeCount = 0;
    
    public Employee(int id, String name, double baseSalary) {
        this.id = id;
        this.name = name;
        this.baseSalary = baseSalary;
        employeeCount++;
    }
    
    // 抽象方法 - 计算总薪资
    public abstract double calculateTotalSalary();
    
    // 抽象方法 - 获取员工类型
    public abstract String getEmployeeType();
    
    // 具体方法
    public void displayInfo() {
        System.out.println("ID: " + id);
        System.out.println("Name: " + name);
        System.out.println("Type: " + getEmployeeType());
        System.out.println("Base Salary: " + baseSalary);
        System.out.println("Total Salary: " + calculateTotalSalary());
    }
    
    public static int getEmployeeCount() {
        return employeeCount;
    }
    
    @Override
    public String toString() {
        return "Employee{id=" + id + ", name='" + name + "', type='" + getEmployeeType() + "'}";
    }
}

// 接口 - 可培训的
interface Trainable {
    void attendTraining(String course);
}

// 接口 - 可管理的
interface Manageable {
    void manageTeam();
    boolean approveLeave(Employee employee, int days);
}

// 全职员工
class FullTimeEmployee extends Employee implements Trainable {
    private double bonus;
    private double allowance;
    
    public FullTimeEmployee(int id, String name, double baseSalary, double bonus, double allowance) {
        super(id, name, baseSalary);
        this.bonus = bonus;
        this.allowance = allowance;
    }
    
    @Override
    public double calculateTotalSalary() {
        return baseSalary + bonus + allowance;
    }
    
    @Override
    public String getEmployeeType() {
        return "Full-Time";
    }
    
    @Override
    public void attendTraining(String course) {
        System.out.println(name + " is attending training: " + course);
    }
    
    public void setBonus(double bonus) {
        this.bonus = bonus;
    }
}

// 兼职员工
class PartTimeEmployee extends Employee {
    private double hourlyRate;
    private int hoursWorked;
    
    public PartTimeEmployee(int id, String name, double baseSalary, double hourlyRate, int hoursWorked) {
        super(id, name, baseSalary);
        this.hourlyRate = hourlyRate;
        this.hoursWorked = hoursWorked;
    }
    
    @Override
    public double calculateTotalSalary() {
        return baseSalary + (hourlyRate * hoursWorked);
    }
    
    @Override
    public String getEmployeeType() {
        return "Part-Time";
    }
    
    public void setHoursWorked(int hoursWorked) {
        this.hoursWorked = hoursWorked;
    }
}

// 经理
class Manager extends FullTimeEmployee implements Manageable {
    private String department;
    private List<Employee> teamMembers;
    
    public Manager(int id, String name, double baseSalary, double bonus, double allowance, String department) {
        super(id, name, baseSalary, bonus, allowance);
        this.department = department;
        this.teamMembers = new ArrayList<>();
    }
    
    @Override
    public double calculateTotalSalary() {
        // 经理有额外的管理津贴
        return super.calculateTotalSalary() + 5000;
    }
    
    @Override
    public String getEmployeeType() {
        return "Manager";
    }
    
    @Override
    public void manageTeam() {
        System.out.println(name + " is managing the " + department + " department.");
        System.out.println("Team members: " + teamMembers.size());
    }
    
    @Override
    public boolean approveLeave(Employee employee, int days) {
        System.out.println(name + " is reviewing leave request for " + employee.name);
        return days <= 5; // 最多批准5天
    }
    
    public void addTeamMember(Employee employee) {
        teamMembers.add(employee);
    }
    
    public String getDepartment() {
        return department;
    }
}

// 枚举 - 部门
enum Department {
    ENGINEERING("Engineering"),
    MARKETING("Marketing"),
    HR("Human Resources"),
    FINANCE("Finance");
    
    private final String displayName;
    
    Department(String displayName) {
        this.displayName = displayName;
    }
    
    public String getDisplayName() {
        return displayName;
    }
}

// 员工管理系统
class EmployeeManagementSystem {
    private List<Employee> employees;
    
    public EmployeeManagementSystem() {
        this.employees = new ArrayList<>();
    }
    
    public void addEmployee(Employee employee) {
        employees.add(employee);
    }
    
    public void displayAllEmployees() {
        System.out.println("=== All Employees (" + employees.size() + ") ===");
        for (Employee employee : employees) {
            employee.displayInfo();
            System.out.println("------------------------");
        }
    }
    
    public double calculateTotalPayroll() {
        double total = 0;
        for (Employee employee : employees) {
            total += employee.calculateTotalSalary();
        }
        return total;
    }
    
    public void displayManagers() {
        System.out.println("=== Managers ===");
        for (Employee employee : employees) {
            if (employee instanceof Manager) {
                Manager manager = (Manager) employee;
                System.out.println(manager.name + " - " + manager.getDepartment());
            }
        }
    }
}

// 主程序
public class EmployeeSystemDemo {
    public static void main(String[] args) {
        // 创建管理系统
        EmployeeManagementSystem system = new EmployeeManagementSystem();
        
        // 创建员工
        FullTimeEmployee ft1 = new FullTimeEmployee(1, "Alice", 8000, 2000, 1000);
        FullTimeEmployee ft2 = new FullTimeEmployee(2, "Bob", 9000, 2500, 1200);
        PartTimeEmployee pt1 = new PartTimeEmployee(3, "Charlie", 3000, 50, 80);
        Manager manager1 = new Manager(4, "David", 15000, 5000, 2000, Department.ENGINEERING.getDisplayName());
        Manager manager2 = new Manager(5, "Eve", 14000, 4500, 1800, Department.MARKETING.getDisplayName());
        
        // 添加团队成员
        manager1.addTeamMember(ft1);
        manager1.addTeamMember(ft2);
        manager2.addTeamMember(pt1);
        
        // 添加到系统
        system.addEmployee(ft1);
        system.addEmployee(ft2);
        system.addEmployee(pt1);
        system.addEmployee(manager1);
        system.addEmployee(manager2);
        
        // 显示所有员工
        system.displayAllEmployees();
        
        // 显示经理
        system.displayManagers();
        
        // 计算总薪资
        System.out.println("\nTotal Payroll: " + system.calculateTotalPayroll());
        
        // 演示培训
        System.out.println("\n=== Training ===");
        ft1.attendTraining("Java Advanced");
        ft2.attendTraining("Spring Boot");
        
        // 演示管理
        System.out.println("\n=== Management ===");
        manager1.manageTeam();
        boolean approved = manager1.approveLeave(ft1, 3);
        System.out.println("Leave approved: " + approved);
        
        // 演示多态
        System.out.println("\n=== Polymorphism Demo ===");
        Employee[] employees = {ft1, pt1, manager1};
        for (Employee emp : employees) {
            System.out.println(emp.name + " - " + emp.getEmployeeType() + 
                             " - Salary: " + emp.calculateTotalSalary());
        }
        
        System.out.println("\nTotal employees created: " + Employee.getEmployeeCount());
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 类设计原则
1. **单一职责原则**：一个类应该只负责一件事情
2. **开闭原则**：对扩展开放，对修改关闭
3. **里氏替换原则**：子类应该能够替换父类而不改变原有的正确性
4. **接口隔离原则**：使用小的、专门的接口而不是大的、通用的接口
5. **依赖倒置原则**：依赖抽象而不是具体实现

### 封装最佳实践
1. **字段私有化**：使用 private 修饰符，通过 getter/setter 访问
2. **验证输入**：在 setter 中验证输入参数的有效性
3. **不可变对象**：对于不需要修改的对象，考虑使用不可变设计
4. **避免返回可变内部状态**：不要直接返回内部集合或数组的引用

### 继承最佳实践
1. **优先使用组合而非继承**：组合比继承更灵活
2. **设计用于继承或禁止继承**：要么为继承设计并文档化，要么用 final 禁止
3. **不要重写可在构造函数中调用的方法**：可能导致未初始化状态的问题
4. **遵守 equals 和 hashCode 约定**：重写 equals 时必须重写 hashCode

### 多态最佳实践
1. **使用 @Override 注解**：确保正确重写方法
2. **谨慎使用向下转型**：在转型前使用 instanceof 检查
3. **利用接口实现多态**：接口比抽象类更灵活
4. **避免 instanceof 滥用**：可以考虑使用访问者模式等替代方案

### 接口与抽象类选择
1. **需要定义类型行为契约时使用接口**：接口定义"能做什么"
2. **需要在多个子类间共享代码时使用抽象类**：抽象类定义"是什么"
3. **Java 8+ 接口可以有默认方法**：这模糊了接口与抽象类的界限
4. **函数式接口使用 @FunctionalInterface 注解**：确保只有一个抽象方法

### 内部类使用
1. **仅在需要访问外部类的私有成员时使用成员内部类**
2. **不需要访问外部类实例时使用静态内部类**
3. **考虑使用 Lambda 表达式替代简单的匿名内部类**（Java 8+）
4. **避免过度使用内部类**：过度使用会使代码难以理解

### 枚举使用
1. **使用枚举代替常量集合**：枚举提供类型安全
2. **在枚举中添加方法和字段**：枚举可以很强大
3. **使用 EnumSet 和 EnumMap**：专门为枚举优化的集合类
4. **考虑使用策略枚举**：通过枚举实现策略模式

### 常见陷阱
1. **忘记调用 super()**：构造函数第一行必须调用父类构造函数
2. **equals 和 hashCode 不匹配**：会导致集合类行为异常
3. **可变对象作为 equals/hashCode 的依据**：对象放入集合后修改会导致问题
4. **clone() 方法的问题**：建议使用拷贝构造函数或静态工厂方法替代
5. **finalize() 方法的问题**：已过时，避免使用

## 6. 官方文档参考链接

- Java 官方教程 - 面向对象编程：https://docs.oracle.com/javase/tutorial/java/concepts/
- Java 语言规范：https://docs.oracle.com/javase/specs/jls/se17/html/
- Java API 文档：https://docs.oracle.com/en/java/javase/17/docs/api/index.html
- 类与对象：https://docs.oracle.com/javase/tutorial/java/javaOO/classes.html
- 继承：https://docs.oracle.com/javase/tutorial/java/IandI/inheritance.html
- 多态：https://docs.oracle.com/javase/tutorial/java/IandI/polymorphism.html
- 抽象类与接口：https://docs.oracle.com/javase/tutorial/java/IandI/abstract.html
- 内部类：https://docs.oracle.com/javase/tutorial/java/javaOO/nested.html
- 枚举类型：https://docs.oracle.com/javase/tutorial/java/javaOO/enum.html
