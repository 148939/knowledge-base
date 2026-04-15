# Maven 与 Gradle

## 1. 核心概述（官方定义）

### Maven 概述
Maven 是一个项目管理和理解工具，用于项目构建、依赖管理和项目信息管理。Maven 基于项目对象模型（POM），可以从一个中心信息块管理项目的构建、报告和文档。

### Gradle 概述
Gradle 是一个现代化的构建自动化工具，使用 Groovy 或 Kotlin DSL，结合了 Ant 的灵活性和 Maven 的约定优于配置特性。Gradle 支持增量构建，性能优异。

---

## 2. 核心知识点（结构化层级）

### 2.1 Maven 核心概念

#### POM（Project Object Model）
- pom.xml：项目对象模型文件
- 项目基本信息
- 依赖配置
- 构建配置

#### Maven 坐标
- groupId：组织标识
- artifactId：项目标识
- version：版本号
- packaging：打包方式

#### Maven 仓库
- 本地仓库
- 中央仓库
- 远程仓库

#### Maven 生命周期
- clean：清理
- validate：验证
- compile：编译
- test：测试
- package：打包
- verify：验证
- install：安装
- deploy：部署

### 2.2 Maven 依赖管理

#### 依赖范围
- compile：编译范围（默认）
- provided：已提供
- runtime：运行时
- test：测试
- system：系统

#### 依赖传递
- 传递性依赖
- 依赖排除
- 依赖管理

#### 依赖冲突解决
- 最短路径优先
- 声明顺序优先
- 依赖管理优先级

### 2.3 Maven 插件

#### 核心插件
- compiler：编译插件
- surefire：测试插件
- jar：打包插件
- war：WAR 插件
- resources：资源处理

#### 常用插件
- spring-boot-maven-plugin：Spring Boot 插件
- maven-shade-plugin：打包插件
- maven-assembly-plugin：打包插件
- exec-maven-plugin：执行插件

### 2.4 Gradle 核心概念

#### 项目与任务
- Project：项目
- Task：任务
- 任务依赖

#### 构建脚本
- build.gradle：构建脚本
- settings.gradle：设置文件
- gradle.properties：属性文件

#### Gradle 插件
- Java 插件
- Spring Boot 插件
- War 插件
- 自定义插件

### 2.5 Gradle 依赖管理

#### 依赖配置
- implementation：实现依赖
- api：API 依赖
- compileOnly：仅编译
- runtimeOnly：仅运行时
- testImplementation：测试实现

#### 依赖仓库
- mavenCentral()
- google()
- mavenLocal()
- 自定义仓库

#### 依赖版本管理
- 版本约束
- 平台（BOM）
- 版本锁定

### 2.6 Gradle 构建优化

#### 增量构建
- 输入输出检测
- 任务缓存
- 构建缓存

#### 并行构建
- 并行执行
- 守护进程
- 配置缓存

---

## 3. 官方标准语法 / API

### 3.1 Maven POM 语法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>
    
    <name>demo</name>
    <description>Demo project</description>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.0</version>
        <relativePath/>
    </parent>
    
    <properties>
        <java.version>17</java.version>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### 3.2 Maven 命令

```bash
# 编译
mvn compile

# 测试
mvn test

# 打包
mvn package

# 安装到本地仓库
mvn install

# 部署到远程仓库
mvn deploy

# 清理
mvn clean

# 组合命令
mvn clean package
mvn clean install

# 跳过测试
mvn clean package -DskipTests
mvn clean package -Dmaven.test.skip=true

# 指定配置文件
mvn clean package -P prod

# 查看依赖树
mvn dependency:tree

# 查看有效 POM
mvn help:effective-pom
```

### 3.3 Gradle 构建脚本（Groovy）

```groovy
plugins {
    id 'java'
    id 'org.springframework.boot' version '3.0.0'
    id 'io.spring.dependency-management' version '1.1.0'
}

group = 'com.example'
version = '1.0.0'
sourceCompatibility = '17'

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-web'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}

bootJar {
    mainClass = 'com.example.demo.DemoApplication'
}
```

### 3.4 Gradle 构建脚本（Kotlin）

```kotlin
plugins {
    java
    id("org.springframework.boot") version "3.0.0"
    id("io.spring.dependency-management") version "1.1.0"
}

group = "com.example"
version = "1.0.0"
java.sourceCompatibility = JavaVersion.VERSION_17

repositories {
    mavenCentral()
}

dependencies {
    implementation("org.springframework.boot:spring-boot-starter-web")
    testImplementation("org.springframework.boot:spring-boot-starter-test")
}

tasks.withType<Test> {
    useJUnitPlatform()
}
```

### 3.5 Gradle 命令

```bash
# 编译
gradle compileJava

# 测试
gradle test

# 打包
gradle bootJar

# 清理
gradle clean

# 组合命令
gradle clean bootJar

# 跳过测试
gradle clean bootJar -x test

# 查看依赖
gradle dependencies

# 查看项目结构
gradle projects

# 查看任务
gradle tasks

# 使用 Gradle Wrapper
./gradlew clean bootJar
gradlew.bat clean bootJar
```

---

## 4. 官方示例代码（可运行）

### 4.1 Maven 多模块项目

```xml
<!-- pom.xml -->
<project>
    <modelVersion>4.0.0</modelVersion>
    
    <groupId>com.example</groupId>
    <artifactId>parent</artifactId>
    <version>1.0.0</version>
    <packaging>pom</packaging>
    
    <modules>
        <module>common</module>
        <module>service</module>
        <module>web</module>
    </modules>
    
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>com.example</groupId>
                <artifactId>common</artifactId>
                <version>${project.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

### 4.2 Gradle 多模块项目

```groovy
// settings.gradle
rootProject.name = 'demo'
include 'common', 'service', 'web'

// build.gradle
plugins {
    id 'java'
}

allprojects {
    group = 'com.example'
    version = '1.0.0'
    
    repositories {
        mavenCentral()
    }
}

subprojects {
    apply plugin: 'java'
    
    dependencies {
        testImplementation 'org.junit.jupiter:junit-jupiter:5.9.0'
    }
    
    test {
        useJUnitPlatform()
    }
}
```

---

## 5. 官方注意事项 / 最佳实践

### 5.1 Maven 最佳实践

1. **依赖管理**
   - 使用 dependencyManagement 统一管理版本
   - 避免版本冲突
   - 定期更新依赖版本

2. **构建优化**
   - 使用 Maven 守护进程
   - 合理配置本地仓库
   - 使用镜像加速

3. **POM 规范**
   - 保持 POM 简洁
   - 使用 parent POM
   - 合理使用 profile

### 5.2 Gradle 最佳实践

1. **性能优化**
   - 启用构建缓存
   - 使用守护进程
   - 启用并行构建

2. **依赖管理**
   - 使用新版本的依赖配置
   - 使用平台（BOM）管理版本
   - 定期更新依赖

3. **脚本规范**
   - 使用 Kotlin DSL（推荐）
   - 保持脚本简洁
   - 使用插件封装逻辑

---

## 6. 官方文档参考链接

- [Maven 官方文档](https://maven.apache.org/guides/)
- [Maven POM 参考](https://maven.apache.org/pom.html)
- [Gradle 官方文档](https://docs.gradle.org/current/userguide/userguide.html)
- [Spring Boot Maven 插件](https://docs.spring.io/spring-boot/docs/current/maven-plugin/)
- [Spring Boot Gradle 插件](https://docs.spring.io/spring-boot/docs/current/gradle-plugin/)
