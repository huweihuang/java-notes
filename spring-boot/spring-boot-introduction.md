# Spring Boot 入门指南：从零开始构建一个 Java Web 应用

# 1. 什么是 Spring Boot？

对于很多 Java 初学者来说，第一次接触 Java Web 开发时，可能会遇到：

- Tomcat 配置复杂

- XML 配置文件很多

- Maven 依赖冲突

- 启动一个项目步骤繁琐

- 各种框架整合困难

而 Spring Boot 的目标，就是：

> “让 Java 开发变得更简单。”

Spring Boot 是基于 Spring Framework 的快速开发框架，它帮助开发者：

- 快速创建 Web 服务

- 自动完成大量配置

- 内置 Web 服务器（Tomcat）

- 简化数据库访问

- 快速打包和部署

一句话理解：

> Spring Boot = Spring + 自动配置 + 快速开发

---

# 2. Spring Boot 能做什么？

Spring Boot 常用于：

| 场景       | 示例             |
| -------- | -------------- |
| Web 网站   | 后台管理系统         |
| REST API | 微服务接口          |
| 企业系统     | ERP、CRM        |
| 云原生应用    | Kubernetes 微服务 |
| AI 后端    | Agent 服务       |

目前大多数 Java 企业项目，都大量使用 Spring Boot。

---

# 3. 开发前准备

## 3.1. 安装 JDK

建议：

- JDK 17（长期支持）

- 或 JDK 21

验证：

```bash
java -version
```

---

## 3.2. 安装 Maven

验证：

```bash
mvn -version
```

---

## 3.3. 安装 IDE

推荐：

- IntelliJ IDEA

---

# 4. 第一个 Spring Boot 项目

我们实现一个简单接口：

```text
http://localhost:8080/hello
```

返回：

```text
Hello Spring Boot
```

---

## 4.1. 创建项目

方法1：使用 Spring Initializr（推荐）

打开：

```text
https://start.spring.io
```

选择：

| 配置          | 内容          |
| ----------- | ----------- |
| Project     | Maven       |
| Language    | Java        |
| Spring Boot | 最新稳定版       |
| Group       | com.example |
| Artifact    | demo        |
| Packaging   | Jar         |
| Java        | 17          |

依赖选择：

- Spring Web

点击：

```text
GENERATE
```

下载项目后导入 IDEA。

---

## 4.2. 项目结构解析

Spring Boot 项目大概长这样：

```text
demo/
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── com/example/demo
│   │   │       └── DemoApplication.java
│   │   └── resources/
│   │       └── application.yml
├── pom.xml
```

核心文件：

| 文件                   | 作用       |
| -------------------- | -------- |
| pom.xml              | Maven 依赖 |
| DemoApplication.java | 启动类      |
| application.yml      | 配置文件     |

---

## 4.3. pom.xml 解析

这是 Maven 配置文件。

核心内容：

```xml
<dependencies>

    <!-- Web开发 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

</dependencies>
```

这里：

```text
spring-boot-starter-web
```

会自动帮你引入：

- Spring MVC

- Jackson

- Tomcat

- 日志框架

这就是 Spring Boot 的“Starter”思想。

---

## 4.4. 编写启动类

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }

}
```

---

@SpringBootApplication 是什么？

它是三个注解组合：

```java
@SpringBootConfiguration
@EnableAutoConfiguration
@ComponentScan
```

作用：

| 注解                      | 功能        |
| ----------------------- | --------- |
| SpringBootConfiguration | Spring配置类 |
| EnableAutoConfiguration | 自动配置      |
| ComponentScan           | 自动扫描Bean  |

---

## 4.5. 编写第一个接口

创建：

```text
HelloController.java
```

代码：

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "Hello Spring Boot";
    }

}
```

---

## 4.6. 代码解析

### 4.6.1. @RestController

表示：

```text
这是一个Web接口类
```

等价于：

```java
@Controller + @ResponseBody
```

---

### 4.6.2. @GetMapping

表示：

```text
HTTP GET请求
```

访问：

```text
/hello
```

时执行：

```java
hello()
```

---

## 4.7. 启动项目

IDEA 运行：

```java
DemoApplication.main()
```

控制台会看到：

```text
Tomcat started on port(s): 8080
```

说明：

Spring Boot 内置 Tomcat 已启动。

---

## 4.8. 访问接口

浏览器打开：

```text
http://localhost:8080/hello
```

返回：

```text
Hello Spring Boot
```

第一个 Spring Boot 应用完成。

---

# 5. Spring Boot 自动配置原理

这是 Spring Boot 最核心的能力。

传统 Spring：

```text
你需要手动配置：
- Tomcat
- DispatcherServlet
- JSON
- Bean
- MVC
```

Spring Boot：

```text
自动帮你配置
```

例如：

你引入：

```xml
spring-boot-starter-web
```

Spring Boot 自动：

- 启动 Tomcat

- 配置 Spring MVC

- 配置 Jackson JSON

- 注册 DispatcherServlet

---

## 5.1. 自动配置核心：

```java
@EnableAutoConfiguration
```

底层会读取：

```text
META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports
```

自动加载大量配置类。

---

# 6. 配置文件 application.yml

Spring Boot 推荐：

```yaml
server:
  port: 8081
```

修改后：

项目端口变为：

```text
8081
```

---

## 6.1. 数据库配置示例

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/test
    username: root
    password: 123456
```

---

# 7. 返回 JSON

修改 Controller：

```java
package com.example.demo.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.HashMap;
import java.util.Map;

@RestController
public class UserController {

    @GetMapping("/user")
    public Map<String, Object> user() {

        Map<String, Object> result = new HashMap<>();

        result.put("name", "Tom");
        result.put("age", 18);

        return result;
    }

}
```

访问：

```text
http://localhost:8080/user
```

返回：

```json
{
  "name": "Tom",
  "age": 18
}
```

Spring Boot 自动完成：

```text
Java对象 → JSON
```

---

# 8. Spring Boot 常用注解

## 8.1. @Component

普通组件。

---

## 8.2. @Service

业务层。

```java
@Service
public class UserService {

}
```

---

## 8.3. @Repository

数据库层。

---

## 8.4. @Autowired

自动注入对象。

```java
@Autowired
private UserService userService;
```

---

# 9. 三层架构

企业开发常见：

```text
Controller
    ↓
Service
    ↓
Repository
```

---

## 9.1. 示例

### 9.1.1. Controller

```java
@RestController
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/user")
    public String user() {
        return userService.getName();
    }

}
```

---

### 9.1.2. Service

```java
@Service
public class UserService {

    public String getName() {
        return "Tom";
    }

}
```

---

# 10. 连接 MySQL

添加依赖：

```xml
<dependency>
    <groupId>com.mysql</groupId>
    <artifactId>mysql-connector-j</artifactId>
</dependency>
```

---

# 11. 使用 JPA

添加依赖：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

---

## 11.1. 实体类

```java
@Entity
public class User {

    @Id
    private Long id;

    private String name;

}
```

---

## 11.2. Repository

```java
public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

Spring Boot 会自动生成 SQL。

---

# 12. 打包运行

执行：

```bash
mvn clean package
```

生成：

```text
target/demo.jar
```

运行：

```bash
java -jar demo.jar
```

---

# 13. Spring Boot 的核心思想

Spring Boot 的设计核心：

| 思想      | 说明          |
| ------- | ----------- |
| 约定大于配置  | 默认帮你配置      |
| Starter | 场景化依赖       |
| 自动配置    | 减少XML       |
| 内嵌服务器   | 不需要外部Tomcat |

---

# 14. 企业真实开发中怎么用？

企业里通常：

```text
Spring Boot
    +
MySQL
    +
Redis
    +
Kafka
    +
Docker
    +
Kubernetes
```

构建：

- 微服务

- AI平台

- 电商系统

- 支付系统

- 运维平台

# 15. 总结

Spring Boot 是目前 Java 企业开发最主流的框架之一。

它解决了传统 Java Web 开发：

- 配置复杂

- 启动困难

- 整合麻烦

等问题。

作为初学者，你需要先掌握：

1. Controller

2. 配置文件

3. Bean

4. 数据库

5. Maven

随后再逐步学习：

- Spring Cloud

- 微服务

- Docker

- Kubernetes

这样才能真正进入现代 Java 后端开发领域。
