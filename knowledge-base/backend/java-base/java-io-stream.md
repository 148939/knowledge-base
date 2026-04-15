# Java IO 流

## 1. 核心概述（官方定义）

Java I/O（Input/Output）是 Java 平台提供的用于处理输入和输出操作的 API。Java I/O 主要包括字节流和字符流两种类型，以及 NIO（New I/O）提供的非阻塞 I/O 操作。

### 核心概念
- **流（Stream）**：数据的序列
- **输入流**：从源读取数据
- **输出流**：向目标写入数据
- **字节流**：以字节为单位处理数据
- **字符流**：以字符为单位处理数据

---

## 2. 核心知识点（结构化层级）

### 2.1 字节流

#### InputStream（输入字节流）
- FileInputStream：文件输入流
- BufferedInputStream：缓冲输入流
- DataInputStream：数据输入流
- ObjectInputStream：对象输入流
- ByteArrayInputStream：字节数组输入流

#### OutputStream（输出字节流）
- FileOutputStream：文件输出流
- BufferedOutputStream：缓冲输出流
- DataOutputStream：数据输出流
- ObjectOutputStream：对象输出流
- ByteArrayOutputStream：字节数组输出流

### 2.2 字符流

#### Reader（输入字符流）
- FileReader：文件输入流
- BufferedReader：缓冲输入流
- InputStreamReader：字节流转字符流
- StringReader：字符串输入流

#### Writer（输出字符流）
- FileWriter：文件输出流
- BufferedWriter：缓冲输出流
- OutputStreamWriter：字符流转字节流
- StringWriter：字符串输出流
- PrintWriter：打印输出流

### 2.3 标准流

#### System 类
- System.in：标准输入流
- System.out：标准输出流
- System.err：标准错误流

#### 重定向
- System.setIn(InputStream)
- System.setOut(PrintStream)
- System.setErr(PrintStream)

### 2.4 文件操作

#### File 类
- 文件和目录路径表示
- 文件操作（创建、删除、重命名）
- 目录操作（创建、列出内容）
- 文件属性检查（存在、可读、可写）

#### Files 类（Java NIO）
- Files.copy()：复制文件
- Files.move()：移动文件
- Files.delete()：删除文件
- Files.exists()：检查存在
- Files.readAllBytes()：读取所有字节
- Files.readAllLines()：读取所有行

### 2.5 序列化

#### 序列化
- 实现 Serializable 接口
- transient 关键字
- serialVersionUID
- writeObject() 和 readObject()

#### 反序列化
- 读取序列化对象
- 版本兼容性
- 安全注意事项

### 2.6 Java NIO

#### 核心组件
- Buffer：缓冲区
- Channel：通道
- Selector：选择器

#### Buffer
- ByteBuffer
- CharBuffer
- IntBuffer
- LongBuffer
- FloatBuffer
- DoubleBuffer

#### Channel
- FileChannel：文件通道
- SocketChannel：Socket 通道
- ServerSocketChannel：ServerSocket 通道
- DatagramChannel：数据报通道

### 2.7 装饰器模式

#### 流的包装
- 缓冲流包装原始流
- 数据流包装缓冲流
- 对象流包装数据流

#### 常用组合
- FileInputStream → BufferedInputStream → DataInputStream
- FileOutputStream → BufferedOutputStream → DataOutputStream
- FileReader → BufferedReader
- FileWriter → BufferedWriter → PrintWriter

---

## 3. 官方标准语法 / API

### 3.1 字节流读写

```java
import java.io.*;

// FileInputStream 读取文件
public class FileReadExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.txt")) {
            int data;
            while ((data = fis.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// 使用 byte 数组读取
public class FileReadBytesExample {
    public static void main(String[] args) {
        try (FileInputStream fis = new FileInputStream("input.txt")) {
            byte[] buffer = new byte[1024];
            int bytesRead;
            while ((bytesRead = fis.read(buffer)) != -1) {
                System.out.write(buffer, 0, bytesRead);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// FileOutputStream 写入文件
public class FileWriteExample {
    public static void main(String[] args) {
        String content = "Hello, Java IO!";
        try (FileOutputStream fos = new FileOutputStream("output.txt")) {
            fos.write(content.getBytes());
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// BufferedInputStream 和 BufferedOutputStream
public class BufferedStreamExample {
    public static void main(String[] args) {
        try (BufferedInputStream bis = new BufferedInputStream(
                new FileInputStream("input.txt"));
             BufferedOutputStream bos = new BufferedOutputStream(
                new FileOutputStream("output.txt"))) {
            
            byte[] buffer = new byte[8192];
            int bytesRead;
            while ((bytesRead = bis.read(buffer)) != -1) {
                bos.write(buffer, 0, bytesRead);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.2 字符流读写

```java
import java.io.*;

// FileReader 读取文件
public class FileReaderExample {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("input.txt")) {
            int data;
            while ((data = reader.read()) != -1) {
                System.out.print((char) data);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// 使用 char 数组读取
public class FileReadCharsExample {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("input.txt")) {
            char[] buffer = new char[1024];
            int charsRead;
            while ((charsRead = reader.read(buffer)) != -1) {
                System.out.print(buffer, 0, charsRead);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// BufferedReader 按行读取
public class BufferedReaderExample {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(
                new FileReader("input.txt"))) {
            String line;
            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// FileWriter 写入文件
public class FileWriterExample {
    public static void main(String[] args) {
        String content = "Hello, Java Character Streams!";
        try (FileWriter writer = new FileWriter("output.txt")) {
            writer.write(content);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// BufferedWriter 写入文件
public class BufferedWriterExample {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(
                new FileWriter("output.txt"))) {
            writer.write("First line");
            writer.newLine();
            writer.write("Second line");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// PrintWriter 格式化输出
public class PrintWriterExample {
    public static void main(String[] args) {
        try (PrintWriter writer = new PrintWriter(
                new BufferedWriter(new FileWriter("output.txt")))) {
            writer.println("Hello, PrintWriter!");
            writer.printf("Name: %s, Age: %d%n", "Alice", 30);
            writer.printf("Pi: %.2f%n", Math.PI);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.3 数据流

```java
import java.io.*;

// DataOutputStream 写入基本类型
public class DataWriteExample {
    public static void main(String[] args) {
        try (DataOutputStream dos = new DataOutputStream(
                new BufferedOutputStream(
                    new FileOutputStream("data.bin")))) {
            
            dos.writeInt(42);
            dos.writeLong(123456789L);
            dos.writeFloat(3.14f);
            dos.writeDouble(2.71828);
            dos.writeBoolean(true);
            dos.writeUTF("Hello, Data Stream!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// DataInputStream 读取基本类型
public class DataReadExample {
    public static void main(String[] args) {
        try (DataInputStream dis = new DataInputStream(
                new BufferedInputStream(
                    new FileInputStream("data.bin")))) {
            
            int intValue = dis.readInt();
            long longValue = dis.readLong();
            float floatValue = dis.readFloat();
            double doubleValue = dis.readDouble();
            boolean booleanValue = dis.readBoolean();
            String stringValue = dis.readUTF();
            
            System.out.println("Int: " + intValue);
            System.out.println("Long: " + longValue);
            System.out.println("Float: " + floatValue);
            System.out.println("Double: " + doubleValue);
            System.out.println("Boolean: " + booleanValue);
            System.out.println("String: " + stringValue);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.4 对象序列化

```java
import java.io.*;

// 可序列化的类
class Person implements Serializable {
    private static final long serialVersionUID = 1L;
    
    private String name;
    private int age;
    private transient String password; // 不序列化
    
    public Person(String name, int age, String password) {
        this.name = name;
        this.age = age;
        this.password = password;
    }
    
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + 
               ", password='" + password + "'}";
    }
}

// ObjectOutputStream 序列化对象
public class SerializeExample {
    public static void main(String[] args) {
        Person person = new Person("Alice", 30, "secret123");
        
        try (ObjectOutputStream oos = new ObjectOutputStream(
                new FileOutputStream("person.ser"))) {
            oos.writeObject(person);
            System.out.println("Object serialized successfully");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// ObjectInputStream 反序列化对象
public class DeserializeExample {
    public static void main(String[] args) {
        try (ObjectInputStream ois = new ObjectInputStream(
                new FileInputStream("person.ser"))) {
            Person person = (Person) ois.readObject();
            System.out.println("Object deserialized successfully");
            System.out.println(person);
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

### 3.5 File 类

```java
import java.io.File;
import java.io.IOException;

public class FileExample {
    public static void main(String[] args) {
        // 创建 File 对象
        File file = new File("example.txt");
        File directory = new File("mydir");
        
        // 文件操作
        System.out.println("File exists: " + file.exists());
        System.out.println("File can read: " + file.canRead());
        System.out.println("File can write: " + file.canWrite());
        System.out.println("File is directory: " + file.isDirectory());
        System.out.println("File is file: " + file.isFile());
        System.out.println("File name: " + file.getName());
        System.out.println("File path: " + file.getPath());
        System.out.println("File absolute path: " + file.getAbsolutePath());
        System.out.println("File length: " + file.length());
        System.out.println("File last modified: " + file.lastModified());
        
        // 创建文件
        try {
            boolean created = file.createNewFile();
            System.out.println("File created: " + created);
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // 创建目录
        boolean dirCreated = directory.mkdir();
        System.out.println("Directory created: " + dirCreated);
        
        // 列出目录内容
        if (directory.isDirectory()) {
            String[] contents = directory.list();
            if (contents != null) {
                for (String content : contents) {
                    System.out.println(content);
                }
            }
        }
        
        // 删除文件
        boolean deleted = file.delete();
        System.out.println("File deleted: " + deleted);
    }
}
```

### 3.6 Java NIO

```java
import java.io.IOException;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.StandardOpenOption;
import java.util.List;

// 使用 ByteBuffer 和 FileChannel
public class NIOExample {
    public static void main(String[] args) {
        // 写入文件
        String content = "Hello, Java NIO!";
        Path path = Paths.get("nio.txt");
        
        try (FileChannel channel = FileChannel.open(
                path, 
                StandardOpenOption.CREATE, 
                StandardOpenOption.WRITE)) {
            
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            buffer.put(content.getBytes());
            buffer.flip(); // 切换到读模式
            channel.write(buffer);
        } catch (IOException e) {
            e.printStackTrace();
        }
        
        // 读取文件
        try (FileChannel channel = FileChannel.open(
                path, 
                StandardOpenOption.READ)) {
            
            ByteBuffer buffer = ByteBuffer.allocate(1024);
            int bytesRead = channel.read(buffer);
            buffer.flip(); // 切换到读模式
            
            byte[] data = new byte[bytesRead];
            buffer.get(data);
            System.out.println(new String(data));
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

// 使用 Files 工具类
public class FilesExample {
    public static void main(String[] args) {
        Path source = Paths.get("source.txt");
        Path target = Paths.get("target.txt");
        
        try {
            // 写入所有字节
            Files.write(source, "Hello, Files!".getBytes());
            
            // 读取所有字节
            byte[] bytes = Files.readAllBytes(source);
            System.out.println(new String(bytes));
            
            // 读取所有行
            List<String> lines = Files.readAllLines(source);
            for (String line : lines) {
                System.out.println(line);
            }
            
            // 复制文件
            Files.copy(source, target);
            
            // 移动文件
            Path moved = Paths.get("moved.txt");
            Files.move(target, moved);
            
            // 删除文件
            Files.delete(moved);
            
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---

## 4. 官方示例代码（可运行）

### 4.1 文件复制工具

```java
import java.io.*;

public class FileCopyUtil {
    
    // 使用字节流复制文件
    public static void copyFileUsingStreams(File source, File target) 
            throws IOException {
        try (InputStream is = new FileInputStream(source);
             OutputStream os = new FileOutputStream(target)) {
            
            byte[] buffer = new byte[8192];
            int bytesRead;
            while ((bytesRead = is.read(buffer)) != -1) {
                os.write(buffer, 0, bytesRead);
            }
        }
    }
    
    // 使用缓冲流复制文件（推荐）
    public static void copyFileUsingBufferedStreams(File source, File target) 
            throws IOException {
        try (InputStream is = new BufferedInputStream(new FileInputStream(source));
             OutputStream os = new BufferedOutputStream(new FileOutputStream(target))) {
            
            byte[] buffer = new byte[8192];
            int bytesRead;
            while ((bytesRead = is.read(buffer)) != -1) {
                os.write(buffer, 0, bytesRead);
            }
        }
    }
    
    // 使用字符流复制文本文件
    public static void copyTextFile(File source, File target) 
            throws IOException {
        try (Reader reader = new BufferedReader(new FileReader(source));
             Writer writer = new BufferedWriter(new FileWriter(target))) {
            
            char[] buffer = new char[8192];
            int charsRead;
            while ((charsRead = reader.read(buffer)) != -1) {
                writer.write(buffer, 0, charsRead);
            }
        }
    }
    
    // 使用 Java NIO 复制文件（推荐）
    public static void copyFileUsingNIO(File source, File target) 
            throws IOException {
        try (FileChannel sourceChannel = new FileInputStream(source).getChannel();
             FileChannel targetChannel = new FileOutputStream(target).getChannel()) {
            
            sourceChannel.transferTo(0, sourceChannel.size(), targetChannel);
        }
    }
    
    public static void main(String[] args) {
        File source = new File("source.txt");
        File target1 = new File("target1.txt");
        File target2 = new File("target2.txt");
        File target3 = new File("target3.txt");
        File target4 = new File("target4.txt");
        
        try {
            System.out.println("Copying using streams...");
            copyFileUsingStreams(source, target1);
            
            System.out.println("Copying using buffered streams...");
            copyFileUsingBufferedStreams(source, target2);
            
            System.out.println("Copying text file...");
            copyTextFile(source, target3);
            
            System.out.println("Copying using NIO...");
            copyFileUsingNIO(source, target4);
            
            System.out.println("All copies completed!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 4.2 日志文件写入器

```java
import java.io.*;
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class LogWriter {
    
    private String logFilePath;
    private DateTimeFormatter formatter;
    
    public LogWriter(String logFilePath) {
        this.logFilePath = logFilePath;
        this.formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    }
    
    public void log(String level, String message) {
        String timestamp = LocalDateTime.now().format(formatter);
        String logLine = String.format("[%s] [%s] %s%n", 
            timestamp, level, message);
        
        try (BufferedWriter writer = new BufferedWriter(
                new FileWriter(logFilePath, true))) {
            writer.write(logLine);
        } catch (IOException e) {
            System.err.println("Failed to write log: " + e.getMessage());
        }
    }
    
    public void info(String message) {
        log("INFO", message);
    }
    
    public void warn(String message) {
        log("WARN", message);
    }
    
    public void error(String message) {
        log("ERROR", message);
    }
    
    public void debug(String message) {
        log("DEBUG", message);
    }
    
    public static void main(String[] args) {
        LogWriter logger = new LogWriter("app.log");
        
        logger.info("Application started");
        logger.debug("Initializing modules");
        logger.warn("Low memory warning");
        logger.error("Database connection failed");
        logger.info("Application shutdown");
    }
}
```

### 4.3 配置文件读写

```java
import java.io.*;
import java.util.HashMap;
import java.util.Map;
import java.util.Properties;

public class ConfigManager {
    
    private Properties properties;
    private String configFile;
    
    public ConfigManager(String configFile) {
        this.configFile = configFile;
        this.properties = new Properties();
        load();
    }
    
    private void load() {
        File file = new File(configFile);
        if (file.exists()) {
            try (InputStream is = new FileInputStream(file)) {
                properties.load(is);
            } catch (IOException e) {
                System.err.println("Failed to load config: " + e.getMessage());
            }
        }
    }
    
    public String get(String key) {
        return properties.getProperty(key);
    }
    
    public String get(String key, String defaultValue) {
        return properties.getProperty(key, defaultValue);
    }
    
    public int getInt(String key, int defaultValue) {
        String value = properties.getProperty(key);
        if (value != null) {
            try {
                return Integer.parseInt(value);
            } catch (NumberFormatException e) {
                return defaultValue;
            }
        }
        return defaultValue;
    }
    
    public boolean getBoolean(String key, boolean defaultValue) {
        String value = properties.getProperty(key);
        if (value != null) {
            return Boolean.parseBoolean(value);
        }
        return defaultValue;
    }
    
    public void set(String key, String value) {
        properties.setProperty(key, value);
    }
    
    public void save() {
        try (OutputStream os = new FileOutputStream(configFile)) {
            properties.store(os, "Application Configuration");
        } catch (IOException e) {
            System.err.println("Failed to save config: " + e.getMessage());
        }
    }
    
    public Map<String, String> getAll() {
        Map<String, String> result = new HashMap<>();
        for (String key : properties.stringPropertyNames()) {
            result.put(key, properties.getProperty(key));
        }
        return result;
    }
    
    public static void main(String[] args) {
        ConfigManager config = new ConfigManager("app.properties");
        
        // 设置配置
        config.set("app.name", "MyApplication");
        config.set("app.version", "1.0.0");
        config.set("database.url", "jdbc:mysql://localhost:3306/mydb");
        config.set("database.port", "3306");
        config.set("debug", "true");
        
        // 保存配置
        config.save();
        
        // 读取配置
        System.out.println("App Name: " + config.get("app.name"));
        System.out.println("App Version: " + config.get("app.version"));
        System.out.println("Database Port: " + config.getInt("database.port", 3306));
        System.out.println("Debug Mode: " + config.getBoolean("debug", false));
        
        // 获取所有配置
        System.out.println("\nAll Config:");
        for (Map.Entry<String, String> entry : config.getAll().entrySet()) {
            System.out.println(entry.getKey() + " = " + entry.getValue());
        }
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 流的关闭

1. **使用 try-with-resources**
   - 自动关闭资源
   - 避免资源泄漏
   - 代码更简洁

2. **正确处理异常**
   - 捕获 IOException
   - 记录异常日志
   - 提供友好的错误信息

3. **finally 块关闭（旧方式）**
   - 检查流是否为 null
   - 处理 close() 异常
   - 推荐使用 try-with-resources

### 5.2 性能优化

1. **使用缓冲流**
   - 减少 I/O 操作次数
   - 提高读写性能
   - 合理设置缓冲区大小

2. **选择合适的流**
   - 二进制文件使用字节流
   - 文本文件使用字符流
   - 大文件使用 NIO

3. **避免频繁 flush**
   - 频繁 flush 影响性能
   - 只在必要时 flush
   - 缓冲流自动管理

### 5.3 字符编码

1. **指定编码**
   - 使用 InputStreamReader/OutputStreamWriter
   - 明确指定字符编码
   - 避免平台相关编码

2. **常见编码**
   - UTF-8（推荐）
   - GBK
   - ISO-8859-1

3. **编码注意事项**
   - 读写使用相同编码
   - 处理 BOM（字节顺序标记）
   - 注意特殊字符

### 5.4 文件操作注意事项

1. **路径处理**
   - 使用 File 或 Path
   - 注意路径分隔符
   - 相对路径 vs 绝对路径

2. **文件权限**
   - 检查文件是否可读写
   - 处理权限不足异常
   - 避免删除重要文件

3. **并发访问**
   - 多线程读写注意同步
   - 使用 FileChannel 锁定
   - 考虑使用数据库

### 5.5 序列化注意事项

1. **serialVersionUID**
   - 显式声明 serialVersionUID
   - 版本变更时更新
   - 保持兼容性

2. **transient 关键字**
   - 不序列化敏感字段
   - 不序列化临时字段
   - 注意反序列化后的值

3. **安全考虑**
   - 不信任输入流
   - 验证反序列化对象
   - 考虑使用安全替代品

### 5.6 NIO 使用建议

1. **缓冲区管理**
   - 合理分配缓冲区大小
   - 正确使用 flip()
   - 重用缓冲区

2. **通道使用**
   - FileChannel 用于文件操作
   - SocketChannel 用于网络
   - 非阻塞模式使用 Selector

3. **性能优势**
   - 大文件传输使用 transferTo
   - 内存映射文件（MappedByteBuffer）
   - 非阻塞 I/O 提升吞吐量

---

## 6. 官方文档参考链接

- [Java I/O 官方文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/package-summary.html)
- [Java NIO 官方文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/nio/package-summary.html)
- [Java I/O 教程](https://docs.oracle.com/javase/tutorial/essential/io/)
- [File 类文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/File.html)
- [Files 类文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/nio/file/Files.html)
- [Serializable 文档](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/io/Serializable.html)
