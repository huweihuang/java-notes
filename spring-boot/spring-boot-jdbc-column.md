# Spring Boot JPA 中 Java 字段如何映射 MySQL 字段

# 1. 为什么需要字段映射？

Java 命名习惯：

```java
userName
createTime
```

通常：

```text
驼峰命名
```

---

MySQL 常见：

```sql
user_name
create_time
```

通常：

```text
下划线命名
```

因此：

```text
Java字段名 ≠ MySQL字段名
```

需要映射关系。

---

# 2. 最基础映射示例

## 2.1. MySQL 表

```sql
CREATE TABLE user (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_name VARCHAR(100),
    create_time DATETIME
);
```

---

# 3. JPA Entity 写法

```java
@Entity
@Table(name = "user")
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_name")
    private String userName;

    @Column(name = "create_time")
    private LocalDateTime createTime;

}
```

---

# 4. 核心注解说明

## 4.1. @Table

指定数据库表名。

```java
@Table(name = "user")
```

表示：

```text
Java类 User
    ↓
映射到
    ↓
MySQL user表
```

---

## 4.2. @Column

指定数据库字段名。

```java
@Column(name = "user_name")
```

表示：

```text
Java字段：
userName

对应：

MySQL字段：
user_name
```

---

# 5. JPA 如何完成映射？

Spring Boot 启动时：

```text
扫描@Entity
    ↓
解析@Column
    ↓
生成映射关系
```

Hibernate 内部会保存：

| Java字段     | MySQL字段     |
| ---------- | ----------- |
| userName   | user_name   |
| createTime | create_time |

---

# 6. 最终 SQL 如何生成？

例如：

```java
userRepository.save(user);
```

Hibernate 自动生成：

```sql
insert into user
(user_name, create_time)
values (?, ?)
```

而不是：

```sql
userName
```

因为：

```text
@Column 已经告诉 Hibernate 映射关系
```

---

# 7. 不写 @Column 会怎么样？

例如：

```java
private String userName;
```

没写：

```java
@Column
```

---

# 8. 默认命名策略

Hibernate 默认会自动：

```text
驼峰 → 下划线
```

即：

| Java       | MySQL       |
| ---------- | ----------- |
| userName   | user_name   |
| createTime | create_time |

因此：

很多情况下：

```java
@Column
```

可以不写。

---

# 9. Spring Boot 默认命名策略

Spring Boot 默认：

```text
SpringPhysicalNamingStrategy
```

会自动转换：

```text
userName
    ↓
user_name
```

---

# 10. 什么时候必须写 @Column？

以下情况必须写：

---

## 10.1. 字段名不同

例如：

```sql
nick_name
```

Java：

```java
private String name;
```

必须：

```java
@Column(name = "nick_name")
private String name;
```

---

## 10.2. SQL关键字

例如：

```sql
desc
order
```

---

## 10.3. 历史数据库

老系统字段：

```sql
usr_nm
crt_tm
```

必须手动映射。

---

# 11. 完整企业级示例

# 12. MySQL 表

```sql
CREATE TABLE t_user (
    user_id BIGINT PRIMARY KEY AUTO_INCREMENT,
    user_name VARCHAR(100),
    user_age INT,
    create_time DATETIME
);
```

---

# 13. Entity

```java
package com.example.demo.entity;

import jakarta.persistence.*;
import java.time.LocalDateTime;

@Entity
@Table(name = "t_user")
public class User {

    @Id
    @Column(name = "user_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(name = "user_name")
    private String userName;

    @Column(name = "user_age")
    private Integer age;

    @Column(name = "create_time")
    private LocalDateTime createTime;

    public Long getId() {
        return id;
    }

    public String getUserName() {
        return userName;
    }

    public Integer getAge() {
        return age;
    }

    public LocalDateTime getCreateTime() {
        return createTime;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public void setCreateTime(LocalDateTime createTime) {
        this.createTime = createTime;
    }
}
```

---

# 14. SQL 执行效果

保存：

```java
userRepository.save(user);
```

Hibernate 自动生成：

```sql
insert into t_user
(user_name, user_age, create_time)
values (?, ?, ?)
```

---

# 15. 查询如何映射？

查询：

```java
userRepository.findAll();
```

SQL：

```sql
select
    user_id,
    user_name,
    user_age,
    create_time
from t_user
```

Hibernate 自动：

```text
MySQL字段
    ↓
转换
    ↓
Java对象字段
```

---

# 16. JPA 类型映射

JPA 自动处理：

| Java类型        | MySQL类型  |
| ------------- | -------- |
| String        | varchar  |
| Integer       | int      |
| Long          | bigint   |
| LocalDateTime | datetime |
| Boolean       | tinyint  |
| BigDecimal    | decimal  |

---

# 17. 字段约束配置

@Column 还能配置：

```java
@Column(
    name = "user_name",
    nullable = false,
    unique = true,
    length = 100
)
private String userName;
```

---

# 18. 含义

| 参数       | 作用       |
| -------- | -------- |
| nullable | 是否允许NULL |
| unique   | 是否唯一     |
| length   | 字段长度     |

---

# 19. 生成 SQL 效果

```sql
user_name varchar(100) not null unique
```

---

# 20. 主键映射

## 20.1. 主键

```java
@Id
```

---

## 20.2. 自增

```java
@GeneratedValue(strategy = GenerationType.IDENTITY)
```

对应：

```sql
AUTO_INCREMENT
```

---

# 21. 时间字段自动填充

企业常见：

```sql
create_time
update_time
```

---

# 22. JPA 自动时间

```java
@CreationTimestamp
@Column(name = "create_time")
private LocalDateTime createTime;
```

---

更新：

```java
@UpdateTimestamp
@Column(name = "update_time")
private LocalDateTime updateTime;
```

Hibernate 自动赋值。

---

# 23. 忽略字段

不想入库：

```java
@Transient
private String tempData;
```

表示：

```text
不映射数据库
```

---

# 24. 枚举映射

```java
@Enumerated(EnumType.STRING)
private UserStatus status;
```

数据库：

```sql
ACTIVE
DISABLED
```

---

# 25. 大对象映射

```java
@Lob
private String content;
```

对应：

```sql
TEXT
LONGTEXT
```

---

# 26. 为什么 JPA 能自动映射？

核心：

```text
反射
```

Hibernate 启动：

```text
读取@Entity
读取@Column
读取字段类型
```

生成：

```text
Entity元数据
```

最终：

```text
Java对象 ←→ SQL
```

自动转换。

---

# 27. Hibernate 内部做了什么？

Hibernate 内部维护：

```text
ClassMetadata
```

类似：

```text
User.userName
    ↓
映射
user.user_name
```

执行 SQL 时自动转换。

---

# 28. 生产环境最佳实践

推荐：

---

## 28.1. Java 使用驼峰

```java
userName
```

---

## 28.2. MySQL 使用下划线

```sql
user_name
```

---

## 28.3. 尽量显式写 @Column

虽然可以自动转换：

但企业推荐：

```java
@Column(name = "user_name")
```

原因：

- 可读性高

- 避免命名策略变化

- 老项目兼容

- 团队统一

---

# 29. JPA 自动建表

如果：

```yaml
spring:
  jpa:
    hibernate:
      ddl-auto: update
```

Hibernate 会自动：

```text
根据Entity生成表结构
```

---

# 30. Entity → SQL 示例

Entity：

```java
@Column(name = "user_name", length = 50)
private String userName;
```

生成：

```sql
user_name varchar(50)
```

---

# 31. 总结

JPA 字段映射核心：

| 注解              | 作用    |
| --------------- | ----- |
| @Entity         | 标记实体类 |
| @Table          | 指定表名  |
| @Column         | 指定字段名 |
| @Id             | 主键    |
| @GeneratedValue | 自增策略  |

---

最终：

```text
Java对象
    ↓
Hibernate ORM映射
    ↓
SQL
    ↓
MySQL
```

开发者：

```text
操作Java对象
```

Hibernate：

```text
自动完成字段映射与SQL转换
```

这就是 JPA ORM 的核心思想。
