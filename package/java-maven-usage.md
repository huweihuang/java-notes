# 1. Maven概念

## 1.1. 什么是 Maven

Apache Maven 是 Java 生态中最常用的包管理工具之一。

它主要解决三个问题：

1. **依赖管理**
   - 自动下载 Java 包（Jar 包）
   - 自动解决依赖之间的依赖
2. **项目构建**
   - 编译代码
   - 打包 Jar / War
   - 运行测试
3. **统一项目结构**
   - 让团队项目目录一致
   - 降低维护成本



## 1.2. 为什么需要 Maven

假设你需要使用：

- MySQL 驱动
- Spring Boot
- Lombok
- Hutool

如果不用 Maven：

- 需要自己找 Jar 包
- 手动下载
- 手动导入
- 版本容易冲突

用了 Maven：

只需要写几行配置：

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.33</version>
</dependency>Maven 会自动：
```

- 下载
- 安装
- 管理依赖关系



## 1.3. Maven 核心概念

### 1. 仓库（Repository）

Maven 的 Jar 包来源。

#### （1）本地仓库

电脑本地缓存目录：

```
~/.m2/repository
```

作用：

- 缓存下载过的依赖
- 避免重复下载



#### （2）中央仓库

Maven 官方仓库：

[Maven Central Repository](https://mvnrepository.com?utm_source=chatgpt.com)

可以搜索各种 Java 包。



#### （3）私服

公司内部仓库。

常见：

- Nexus
- Artifactory

作用：

- 加速下载
- 管理内部组件



# 2. Maven的使用

## 2.1. Maven 项目标准目录结构

```bash
project-name
│
├── src
│   ├── main
│   │   ├── java        Java源码
│   │   └── resources   配置文件
│   │
│   └── test
│       ├── java        测试代码
│       └── resources   测试资源
│
├── pom.xml             Maven核心配置文件
│
└── target              编译输出目录
```

## 2.2. pom.xml 是什么

`pom.xml` 是 Maven 的核心配置文件。

POM：

```
Project Object Model项目对象模型
```

**pom.xml 示例**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="
         http://maven.apache.org/POM/4.0.0
         http://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <!-- 项目坐标 -->
    <groupId>com.demo</groupId>
    <artifactId>maven-demo</artifactId>
    <version>1.0-SNAPSHOT</version>

</project>
```

## 2.3. Maven 坐标（重点）

每个 Jar 包都有唯一坐标。

格式：

```
groupId + artifactId + version
```

例如：

```xml
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-starter-web</artifactId>
<version>3.3.0</version>
```

| 字段         | 含义    |
| - | -- |
| groupId    | 公司/组织 |
| artifactId | 项目名   |
| version    | 版本号   |

## 2.4. 添加依赖（最重要）

maven需要**显示的声明依赖**，并编辑pom.xml的文件声明所需要的依赖，执行mvn install下载依赖。

示例：添加 MySQL 驱动

```xml
<dependencies>

    <dependency>
        <groupId>com.mysql</groupId>
        <artifactId>mysql-connector-j</artifactId>
        <version>8.3.0</version>
    </dependency>

</dependencies>
```

执行安装依赖包：

```bash
mvn install
```

**删除依赖**

如果需要删除未使用的依赖，可以执行依赖分析的命令。

```bash
mvn dependency:analyze
```

修改pom.xml的文件删除依赖。

**查看依赖树**

```bash
mvn dependency:tree
```

# 3. Maven 常用命令

## 3.1. 理解 Maven 生命周期

Maven 默认生命周期：

```bash
validate
compile
test
package
verify
install
deploy
```

特点

```
后面的阶段会自动执行前面的阶段
```

例如：

```bash
mvn install
```

实际上会执行：

```bash
validate
→ compile
→ test
→ package
→ install
```

## 3.2. 常用命令

```bash
# 只编译生成class的文件
mvn clean compile

# 编译并生成jar包：编译 + 测试 + 打包
mvn clean package

# 下载依赖：编译 + 测试 + 打包 + 安装
mvn clean install

# 跳过测试
mvn clean package -DskipTests
mvn clean install -DskipTests

# 强制更新依赖
mvn clean install -U

# 查看依赖树
mvn dependency:tree

# 分析无用依赖
mvn dependency:analyze

# 查看可升级版本
mvn versions:display-dependency-updates
```

**常见命令的区别**

| 命令            | 编译  | 测试  | 打包  | 安装本地仓库 |
| - |  |  |  |  |
| clean compile | ✅   | ❌   | ❌   | ❌      |
| clean package | ✅   | ✅   | ✅   | ❌      |
| clean install | ✅   | ✅   | ✅   | ✅      |

# 4. 依赖冲突

## 4.1. 什么是依赖冲突

A 依赖：

```
log4j 1.0
```

B 依赖：

```
log4j 2.0
```

项目同时引用：

可能冲突。



## 4.2. Maven 如何解决

Maven 默认规则：

### 4.2.1. 路径最近优先（最重要）

例如：

```bash
Project
 ├── A
 │    └── log4j 1.0
 │
 └── B
      └── C
           └── log4j 2.0
```

由于：

```
A → log4j
```

路径更短。

所以最终：

```
log4j 1.0 生效
```

### 4.2.2. 同层级先声明优先

例如：

```
<dependency>    A</dependency><dependency>    B</dependency>
```

如果：

```
A 和 B
```

距离一样：

```
谁先写谁赢## 
```

## 4.3. 查看依赖树

```bash
mvn dependency:tree
```

示例：

```bash
[INFO] +- spring-boot-starter-web
[INFO] |  \- jackson-databind:2.17
[INFO]
[INFO] \- old-sdk
[INFO]    \- jackson-databind:2.9
```

可以直接看出：

```
版本冲突
```


