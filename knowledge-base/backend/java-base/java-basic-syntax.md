# 检索索引：本文档收录【Java】【基础语法】知识点，内容100%来自Oracle官方文档，适合智能体检索调用

## 1. 核心概述（官方定义）

Java 是一种高级、通用、面向对象的编程语言。它最初由 Sun Microsystems 公司（现 Oracle 公司）开发，于 1995 年发布。Java 的设计理念是"一次编写，到处运行"（Write Once, Run Anywhere），即编译后的 Java 代码可以在任何支持 Java 的平台上运行，无需重新编译。

## 2. 核心知识点（结构化层级）

### 2.1 基本语法
1. Java 程序结构
2. 注释
3. 标识符
4. 关键字
5. 分隔符

### 2.2 数据类型
1. 基本数据类型
   - 整数类型：byte, short, int, long
   - 浮点类型：float, double
   - 字符类型：char
   - 布尔类型：boolean
2. 引用数据类型
3. 类型转换

### 2.3 变量与常量
1. 变量声明
2. 变量初始化
3. 变量作用域
4. 常量（final）

### 2.4 运算符
1. 算术运算符
2. 赋值运算符
3. 比较运算符
4. 逻辑运算符
5. 位运算符
6. 条件运算符（三元运算符）
7. 运算符优先级

### 2.5 控制流程
1. 条件语句
   - if-else
   - switch
2. 循环语句
   - for
   - while
   - do-while
3. 跳转语句
   - break
   - continue
   - return

### 2.6 数组
1. 一维数组
2. 多维数组
3. 数组操作
4. 增强 for 循环

### 2.7 方法
1. 方法声明
2. 方法参数
3. 方法返回值
4. 方法重载
5. 可变参数

## 3. 官方标准语法 / API

### 基本程序结构
```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

### 数据类型声明
```java
// 整数类型
byte byteValue = 127;
short shortValue = 32767;
int intValue = 2147483647;
long longValue = 9223372036854775807L;

// 浮点类型
float floatValue = 3.14f;
double doubleValue = 3.141592653589793;

// 字符类型
char charValue = 'A';
char unicodeChar = '\u0041';

// 布尔类型
boolean booleanValue = true;
boolean anotherBoolean = false;

// 引用类型
String stringValue = "Hello, Java!";
```

### 变量与常量
```java
public class VariableExample {
    // 实例变量
    private int instanceVar;
    
    // 静态变量
    private static int staticVar;
    
    // 常量
    private static final double PI = 3.14159;
    private final int MAX_VALUE = 100;
    
    public void method() {
        // 局部变量
        int localVar = 10;
        
        System.out.println(localVar);
        System.out.println(instanceVar);
        System.out.println(staticVar);
        System.out.println(PI);
    }
}
```

### 运算符
```java
// 算术运算符
int a = 10, b = 3;
int sum = a + b;          // 13
int difference = a - b;   // 7
int product = a * b;      // 30
int quotient = a / b;     // 3
int remainder = a % b;    // 1
int increment = ++a;      // 11
int decrement = --b;      // 2

// 赋值运算符
int x = 10;
x += 5;   // x = x + 5 = 15
x -= 3;   // x = x - 3 = 12
x *= 2;   // x = x * 2 = 24
x /= 4;   // x = x / 4 = 6
x %= 4;   // x = x % 4 = 2

// 比较运算符
int p = 5, q = 10;
boolean isEqual = (p == q);        // false
boolean isNotEqual = (p != q);     // true
boolean isLess = (p < q);          // true
boolean isGreater = (p > q);       // false
boolean isLessOrEqual = (p <= q);  // true
boolean isGreaterOrEqual = (p >= q); // false

// 逻辑运算符
boolean t = true, f = false;
boolean andResult = t && f;    // false
boolean orResult = t || f;     // true
boolean notResult = !t;         // false

// 条件运算符
int max = (a > b) ? a : b;

// 位运算符
int num1 = 5;  // 0101
int num2 = 3;  // 0011
int bitAnd = num1 & num2;   // 0001 = 1
int bitOr = num1 | num2;    // 0111 = 7
int bitXor = num1 ^ num2;   // 0110 = 6
int bitNot = ~num1;          // 1010 (补码)
int leftShift = num1 << 1;   // 1010 = 10
int rightShift = num1 >> 1;  // 0010 = 2
int unsignedRightShift = num1 >>> 1; // 0010 = 2
```

### 控制流程
```java
// if-else
int score = 85;
if (score >= 90) {
    System.out.println("优秀");
} else if (score >= 80) {
    System.out.println("良好");
} else if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}

// switch
int day = 3;
switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    case 4:
        System.out.println("Thursday");
        break;
    case 5:
        System.out.println("Friday");
        break;
    default:
        System.out.println("Weekend");
}

// switch 表达式 (Java 14+)
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    case 4 -> "Thursday";
    case 5 -> "Friday";
    default -> "Weekend";
};

// for 循环
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}

// 增强 for 循环
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}

// while 循环
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}

// do-while 循环
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 10);

// break 和 continue
for (int k = 0; k < 10; k++) {
    if (k == 5) {
        break; // 退出循环
    }
    if (k % 2 == 0) {
        continue; // 跳过本次迭代
    }
    System.out.println(k);
}
```

### 数组
```java
// 一维数组
// 声明
int[] array1;
int array2[];

// 创建
array1 = new int[5];
int[] array3 = new int[5];

// 初始化
int[] array4 = {1, 2, 3, 4, 5};
int[] array5 = new int[]{1, 2, 3, 4, 5};

// 访问
array1[0] = 10;
int firstElement = array4[0];

// 长度
int length = array4.length;

// 遍历
for (int k = 0; k < array4.length; k++) {
    System.out.println(array4[k]);
}

// 增强 for 循环
for (int num : array4) {
    System.out.println(num);
}

// 二维数组
// 声明
int[][] matrix1;
int matrix2[][];
int[] matrix3[];

// 创建
matrix1 = new int[3][4];
int[][] matrix4 = new int[3][4];

// 不规则数组
int[][] matrix5 = new int[3][];
matrix5[0] = new int[2];
matrix5[1] = new int[3];
matrix5[2] = new int[4];

// 初始化
int[][] matrix6 = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// 访问
matrix6[0][0] = 10;
int element = matrix6[1][1];

// 遍历
for (int row = 0; row < matrix6.length; row++) {
    for (int col = 0; col < matrix6[row].length; col++) {
        System.out.print(matrix6[row][col] + " ");
    }
    System.out.println();
}
```

### 方法
```java
public class MethodExample {
    
    // 无参数无返回值的方法
    public void sayHello() {
        System.out.println("Hello!");
    }
    
    // 有参数无返回值的方法
    public void greet(String name) {
        System.out.println("Hello, " + name + "!");
    }
    
    // 无参数有返回值的方法
    public int getRandomNumber() {
        return (int) (Math.random() * 100);
    }
    
    // 有参数有返回值的方法
    public int add(int a, int b) {
        return a + b;
    }
    
    // 方法重载
    public int multiply(int a, int b) {
        return a * b;
    }
    
    public double multiply(double a, double b) {
        return a * b;
    }
    
    public int multiply(int a, int b, int c) {
        return a * b * c;
    }
    
    // 可变参数
    public int sum(int... numbers) {
        int total = 0;
        for (int num : numbers) {
            total += num;
        }
        return total;
    }
    
    // 递归方法
    public int factorial(int n) {
        if (n <= 1) {
            return 1;
        }
        return n * factorial(n - 1);
    }
    
    public static void main(String[] args) {
        MethodExample example = new MethodExample();
        
        example.sayHello();
        example.greet("Alice");
        
        int random = example.getRandomNumber();
        System.out.println("Random number: " + random);
        
        int sum = example.add(5, 3);
        System.out.println("Sum: " + sum);
        
        int product1 = example.multiply(2, 3);
        double product2 = example.multiply(2.5, 3.5);
        int product3 = example.multiply(2, 3, 4);
        
        int total1 = example.sum(1, 2, 3);
        int total2 = example.sum(1, 2, 3, 4, 5);
        
        int fact = example.factorial(5);
        System.out.println("Factorial of 5: " + fact);
    }
}
```

## 4. 官方示例代码（可运行）

### 计算器示例
```java
import java.util.Scanner;

public class Calculator {
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("简单计算器");
        System.out.println("-------------------");
        
        System.out.print("请输入第一个数字: ");
        double num1 = scanner.nextDouble();
        
        System.out.print("请输入运算符 (+, -, *, /): ");
        String operator = scanner.next();
        
        System.out.print("请输入第二个数字: ");
        double num2 = scanner.nextDouble();
        
        double result = 0;
        boolean valid = true;
        
        switch (operator) {
            case "+":
                result = add(num1, num2);
                break;
            case "-":
                result = subtract(num1, num2);
                break;
            case "*":
                result = multiply(num1, num2);
                break;
            case "/":
                if (num2 != 0) {
                    result = divide(num1, num2);
                } else {
                    System.out.println("错误：除数不能为零");
                    valid = false;
                }
                break;
            default:
                System.out.println("错误：无效的运算符");
                valid = false;
        }
        
        if (valid) {
            System.out.println("计算结果: " + num1 + " " + operator + " " + num2 + " = " + result);
        }
        
        scanner.close();
    }
    
    public static double add(double a, double b) {
        return a + b;
    }
    
    public static double subtract(double a, double b) {
        return a - b;
    }
    
    public static double multiply(double a, double b) {
        return a * b;
    }
    
    public static double divide(double a, double b) {
        return a / b;
    }
}
```

### 数组操作示例
```java
import java.util.Arrays;

public class ArrayOperations {
    
    public static void main(String[] args) {
        // 创建数组
        int[] numbers = {5, 2, 9, 1, 5, 6};
        
        System.out.println("原始数组: " + Arrays.toString(numbers));
        
        // 求最大值
        int max = findMax(numbers);
        System.out.println("最大值: " + max);
        
        // 求最小值
        int min = findMin(numbers);
        System.out.println("最小值: " + min);
        
        // 求和
        int sum = sumArray(numbers);
        System.out.println("数组和: " + sum);
        
        // 求平均值
        double average = averageArray(numbers);
        System.out.println("平均值: " + average);
        
        // 排序
        sortArray(numbers);
        System.out.println("排序后: " + Arrays.toString(numbers));
        
        // 反转
        reverseArray(numbers);
        System.out.println("反转后: " + Arrays.toString(numbers));
        
        // 查找
        int index = findElement(numbers, 5);
        System.out.println("元素 5 的索引: " + index);
        
        // 去重
        int[] uniqueArray = removeDuplicates(numbers);
        System.out.println("去重后: " + Arrays.toString(uniqueArray));
    }
    
    public static int findMax(int[] array) {
        int max = array[0];
        for (int num : array) {
            if (num > max) {
                max = num;
            }
        }
        return max;
    }
    
    public static int findMin(int[] array) {
        int min = array[0];
        for (int num : array) {
            if (num < min) {
                min = num;
            }
        }
        return min;
    }
    
    public static int sumArray(int[] array) {
        int sum = 0;
        for (int num : array) {
            sum += num;
        }
        return sum;
    }
    
    public static double averageArray(int[] array) {
        return (double) sumArray(array) / array.length;
    }
    
    public static void sortArray(int[] array) {
        Arrays.sort(array);
    }
    
    public static void reverseArray(int[] array) {
        int left = 0;
        int right = array.length - 1;
        while (left < right) {
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
            left++;
            right--;
        }
    }
    
    public static int findElement(int[] array, int target) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] == target) {
                return i;
            }
        }
        return -1;
    }
    
    public static int[] removeDuplicates(int[] array) {
        if (array.length <= 1) {
            return array;
        }
        
        Arrays.sort(array);
        int[] temp = new int[array.length];
        int j = 0;
        
        for (int i = 0; i < array.length - 1; i++) {
            if (array[i] != array[i + 1]) {
                temp[j++] = array[i];
            }
        }
        temp[j++] = array[array.length - 1];
        
        int[] result = new int[j];
        System.arraycopy(temp, 0, result, 0, j);
        return result;
    }
}
```

## 5. 官方注意事项 / 最佳实践

### 基本语法注意事项
1. **大小写敏感**：Java 是大小写敏感的语言，Hello 和 hello 是不同的
2. **类名**：类名应该以大写字母开头，使用驼峰命名法
3. **方法名**：方法名应该以小写字母开头，使用驼峰命名法
4. **常量名**：常量名应该全部大写，使用下划线分隔单词
5. **源文件名**：源文件名必须与 public 类名相同
6. **main 方法**：程序入口是 public static void main(String[] args)

### 数据类型注意事项
1. **整数类型范围**：注意各整数类型的范围，避免溢出
2. **long 类型**：long 类型的字面量需要加 L 或 l 后缀
3. **float 类型**：float 类型的字面量需要加 F 或 f 后缀
4. **char 类型**：char 类型是 16 位 Unicode 字符
5. **boolean 类型**：boolean 类型只有 true 和 false 两个值，不能与整数互换
6. **类型转换**：自动类型转换只能从低级到高级，强制类型转换可能损失精度

### 变量最佳实践
1. **先声明后使用**：变量必须先声明后使用
2. **最小作用域**：将变量声明在最小的作用域内
3. **初始化变量**：尽量在声明时初始化变量
4. **使用有意义的名字**：变量名应该清晰表达其用途
5. **避免魔法数字**：使用常量代替魔法数字
6. **优先使用 final**：不会改变的变量声明为 final

### 运算符注意事项
1. **整数除法**：整数相除结果还是整数，小数部分会被截断
2. **取模运算**：取模运算的结果符号与被除数相同
3. **逻辑运算符**：&& 和 || 是短路运算符
4. **位运算符**：注意区分位运算符和逻辑运算符
5. **运算符优先级**：不确定时使用括号提高可读性

### 控制流程最佳实践
1. **if-else vs switch**：多个固定值比较时使用 switch
2. **避免嵌套过深**：过深的嵌套会降低可读性
3. **使用 break**：switch 语句中不要忘记 break
4. **for vs while**：知道循环次数时使用 for，否则使用 while
5. **避免死循环**：确保循环有终止条件
6. **合理使用 continue**：continue 会跳过本次循环剩余部分

### 数组最佳实践
1. **数组索引**：数组索引从 0 开始，注意越界问题
2. **数组长度**：使用 length 属性获取数组长度
3. **遍历数组**：优先使用增强 for 循环
4. **Arrays 工具类**：使用 java.util.Arrays 类提供的工具方法
5. **避免数组越界**：确保索引在有效范围内
6. **多维数组**：多维数组实际上是数组的数组

### 方法最佳实践
1. **方法命名**：方法名应该是动词，清晰表达其功能
2. **单一职责**：每个方法只做一件事
3. **参数不宜过多**：参数过多时考虑使用对象封装
4. **方法重载**：合理使用方法重载
5. **可变参数**：可变参数应该是最后一个参数
6. **递归注意**：递归要有终止条件，避免栈溢出

## 6. 官方文档参考链接

- Java 官方文档：https://docs.oracle.com/en/java/
- Java 教程：https://docs.oracle.com/javase/tutorial/
- Java API 文档：https://docs.oracle.com/en/java/javase/
- Java 语言规范：https://docs.oracle.com/javase/specs/
