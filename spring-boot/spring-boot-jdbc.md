# Spring Boot 操作 MySQL 完整入门：从数据库连接到增删改查实战

# 1. 为什么 Spring Boot 要结合 MySQL？

在企业开发中：

```text
程序 = 业务逻辑 + 数据存储
```

而数据通常存放在：

- MySQL

- PostgreSQL

- Oracle

其中：

> MySQL 是 Java 企业开发最常见的数据库。

Spring Boot 则负责：

```text
把 Java 代码 和 MySQL 数据库连接起来
```

实现：

- 新增数据

- 查询数据

- 修改数据

- 删除数据

也就是：

```text
CRUD
(Create Read Update Delete)
```

---

# 2. Spring Boot 操作 MySQL 的几种方式

Spring Boot 操作数据库主要有：

| 方式            | 特点           |
| ------------- | ------------ |
| JDBC          | 原生SQL，较底层    |
| JdbcTemplate  | Spring封装JDBC |
| JPA/Hibernate | ORM自动映射      |
| MyBatis       | SQL灵活，企业最常用  |

---

# 3. 初学者推荐学习路线

建议：

```text
先学 JDBC
再学 JPA
最后学 MyBatis
```

因为：

- JDBC 可以理解数据库底层

- JPA 理解 ORM

- MyBatis 理解企业真实开发

---

# 4. 本文使用什么方案？

本文使用：

```text
Spring Boot + JPA + MySQL
```

原因：

- 最容易入门

- 代码量最少

- Spring Boot 整合最好

---

# 5. 项目目标

实现一个：

```text
用户管理系统
```

支持：

| 功能   | HTTP   |
| ---- | ------ |
| 新增用户 | POST   |
| 查询用户 | GET    |
| 修改用户 | PUT    |
| 删除用户 | DELETE |

数据库：

```text
MySQL
```

---

# 6. 创建数据库

登录 MySQL：

```sql
CREATE DATABASE demo;
```

---

# 7. 创建用户表

```sql
USE demo;

CREATE TABLE user (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    age INT
);
```

---

# 8. 创建 Spring Boot 项目

创建项目时添加依赖：

- Spring Web

- Spring Data JPA

- MySQL Driver

---

# 9. pom.xml 配置

```xml
<dependencies>

    <!-- Web -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <!-- JPA -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-data-jpa</artifactId>
    </dependency>

    <!-- MySQL驱动 -->
    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
    </dependency>

</dependencies>
```

---

# 10. application.yml 配置数据库

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/demo
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver

  jpa:
    hibernate:
      ddl-auto: update

    show-sql: true
```

---

# 11. 配置解析

## 11.1. datasource

用于连接数据库。

---

## 11.2. show-sql

```yaml
show-sql: true
```

会打印 SQL：

```sql
select * from user
```

方便学习。

---

## 11.3. ddl-auto

```yaml
ddl-auto: update
```

表示：

```text
自动创建或更新表结构
```

---

# 12. 什么是 ORM？

ORM：

```text
Object Relational Mapping
对象关系映射
```

意思：

```text
Java对象 ←→ 数据库表
```

例如：

| Java对象 | MySQL  |
| ------ | ------ |
| User类  | user表  |
| 属性name | 字段name |

---

# 13. 创建实体类 Entity

创建：

```text
User.java
```

---

## 13.1. 完整代码

```java
package com.example.demo.entity;

import jakarta.persistence.*;

@Entity
@Table(name = "user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;

    private Integer age;

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }
}
```

---

# 14. Entity 注解详解

## 14.1. @Entity

表示：

```text
这是数据库实体类
```

---

## 14.2. @Table

指定数据库表名：

```java
@Table(name = "user")
```

---

## 14.3. @Id

主键。

---

## 14.4. @GeneratedValue

主键自增。

---

# 15. 创建 Repository

创建：

```text
UserRepository.java
```

---

## 15.1. 完整代码

```java
package com.example.demo.repository;

import com.example.demo.entity.User;
import org.springframework.data.jpa.repository.JpaRepository;

public interface UserRepository
        extends JpaRepository<User, Long> {

}
```

---

# 16. 为什么只写接口就能操作数据库？

这是很多初学者最震惊的地方。

因为：

```text
Spring Data JPA 自动生成实现类
```

---

# 17. JpaRepository 提供什么能力？

已经内置：

| 方法           | 功能    |
| ------------ | ----- |
| save()       | 新增/更新 |
| findAll()    | 查询全部  |
| findById()   | 查询单个  |
| deleteById() | 删除    |

---

# 18. 创建 Service

创建：

```text
UserService.java
```

---

## 18.1. 完整代码

```java
package com.example.demo.service;

import com.example.demo.entity.User;
import com.example.demo.repository.UserRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    // 新增
    public User save(User user) {
        return userRepository.save(user);
    }

    // 查询全部
    public List<User> list() {
        return userRepository.findAll();
    }

    // 删除
    public void delete(Long id) {
        userRepository.deleteById(id);
    }

}
```

---

# 19. 为什么需要 Service？

企业开发通常：

```text
Controller
    ↓
Service
    ↓
Repository
```

原因：

- 业务逻辑分层

- 更容易维护

- 更容易扩展

---

# 20. 创建 Controller

创建：

```text
UserController.java
```

---

# 21. 新增用户接口

```java
@PostMapping("/user")
public User save(@RequestBody User user) {
    return userService.save(user);
}
```

---

# 22. 查询用户接口

```java
@GetMapping("/user")
public List<User> list() {
    return userService.list();
}
```

---

# 23. 删除用户接口

```java
@DeleteMapping("/user/{id}")
public void delete(@PathVariable Long id) {
    userService.delete(id);
}
```

---

# 24. 完整 Controller

```java
package com.example.demo.controller;

import com.example.demo.entity.User;
import com.example.demo.service.UserService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
public class UserController {

    @Autowired
    private UserService userService;

    // 新增
    @PostMapping("/user")
    public User save(@RequestBody User user) {
        return userService.save(user);
    }

    // 查询
    @GetMapping("/user")
    public List<User> list() {
        return userService.list();
    }

    // 删除
    @DeleteMapping("/user/{id}")
    public void delete(@PathVariable Long id) {
        userService.delete(id);
    }
}
```

---

# 25. 启动项目

运行：

```java
DemoApplication.main()
```

控制台：

```text
Tomcat started on port(s): 8080
```

说明项目启动成功。

---

# 26. 测试新增接口

## 26.1. 请求

```http
POST /user
Content-Type: application/json
```

Body：

```json
{
  "name": "Tom",
  "age": 18
}
```

---

## 26.2. 返回

```json
{
  "id": 1,
  "name": "Tom",
  "age": 18
}
```

---

# 27. 数据库发生了什么？

Spring Boot 自动执行：

```sql
insert into user(name, age)
values ('Tom', 18);
```

---

# 28. 测试查询接口

请求：

```http
GET /user
```

返回：

```json
[
  {
    "id": 1,
    "name": "Tom",
    "age": 18
  }
]
```

---

# 29. 底层 SQL

Spring 自动生成：

```sql
select * from user;
```

---

# 30. 测试删除接口

请求：

```http
DELETE /user/1
```

---

# 31. 底层 SQL

```sql
delete from user where id = 1;
```

---

# 32. Spring Boot 操作数据库完整流程

## 32.1. 新增数据流程

```text
HTTP请求
    ↓
Controller
    ↓
Service
    ↓
Repository
    ↓
JPA
    ↓
Hibernate
    ↓
JDBC
    ↓
MySQL
```

---

# 33. Hibernate 是什么？

JPA：

```text
只是规范
```

Hibernate：

```text
是真正执行SQL的实现
```

关系：

```text
Spring Data JPA
    ↓
Hibernate
    ↓
JDBC
    ↓
MySQL
```

---

# 34. JDBC 是什么？

JDBC：

```text
Java Database Connectivity
```

Java 官方数据库接口。

---

# 35. 如果不用 Spring Boot 会多复杂？

传统 JDBC：

```java
Connection conn = DriverManager.getConnection(...);

PreparedStatement ps = conn.prepareStatement(...);

ResultSet rs = ps.executeQuery();
```

还要：

- 手动关闭连接

- 手动转换对象

- 手动写 SQL

代码非常繁琐。

---

# 36. Spring Boot 帮你做了什么？

Spring Boot 自动：

- 数据源连接池

- JDBC连接管理

- SQL执行

- ORM映射

- JSON转换

- 事务管理

开发者只需要：

```java
userRepository.save(user);
```

即可。

---

# 37. 事务 Transaction

## 37.1. 示例

```java
@Transactional
public void transfer() {

}
```

---

# 38. 事务作用

保证：

```text
要么全部成功
要么全部失败
```

例如：

转账：

```text
A减钱
B加钱
```

必须同时成功。

---

# 39. SQL 日志分析

如果：

```yaml
show-sql: true
```

控制台会打印：

```sql
Hibernate:
insert into user (age,name) values (?,?)
```

可以观察 ORM 如何生成 SQL。

---

# 40. JPA 优点与缺点

## 40.1. 优点

- 开发快

- 代码少

- 自动SQL

---

## 40.2. 缺点

- SQL 不够灵活

- 复杂查询性能难优化

---

# 41. 为什么企业大量使用 MyBatis？

因为：

```text
复杂业务最终还是SQL更灵活
```

所以：

很多公司：

```text
Spring Boot + MyBatis
```

而不是 JPA。

---

# 42. 真实企业项目结构

```text
controller/
service/
repository/
entity/
config/
```

---

# 43. 生产环境注意事项

不要：

```yaml
ddl-auto: update
```

用于生产。

原因：

```text
可能误修改表结构
```

生产通常：

- Flyway

- Liquibase

- 手动SQL迁移

---

# 44. Spring Boot + MySQL 最核心知识

初学者必须掌握：

| 知识         | 重要性   |
| ---------- | ----- |
| Entity     | ★★★★★ |
| Repository | ★★★★★ |
| Controller | ★★★★★ |
| Service    | ★★★★★ |
| JPA        | ★★★★  |
| JDBC       | ★★★★★ |
| 事务         | ★★★★★ |

---

# 45. 总结

Spring Boot 操作 MySQL 的核心思想：

```text
Java对象 ←→ ORM ←→ SQL ←→ MySQL
```

开发者：

```text
操作Java对象
```

Spring Boot：

```text
自动转换为SQL
```

底层完整链路：

```text
Spring Boot
    ↓
Spring Data JPA
    ↓
Hibernate
    ↓
JDBC
    ↓
MySQL
```

这也是现代 Java 企业开发最核心的基础能力之一。
