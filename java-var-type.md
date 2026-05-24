# 1. 变量声明

```java
数据类型 变量名 = 初始值;
```

示例：

```java
int age = 18;  
double price = 19.9;  
String name = "Tom";
```

也可以先声明再赋值：

```java
int score;  
score = 90;
```

# 2. 基本数据类型（8种）

Java 一共 **8 种基本类型**

## 1️⃣ 整数类型

| 类型    | 占用字节 | 范围             |
| ----- | ---- | -------------- |
| byte  | 1    | -128 ~ 127     |
| short | 2    | -32768 ~ 32767 |
| int   | 4    | 常用             |
| long  | 8    | 大整数            |

示例：

```java
int age = 18;  
long population = 8000000000L; // L必须写
```

默认整数类型是 **int**

## 2️⃣ 浮点类型

| 类型     | 占用字节 | 精度      |
| ------ | ---- | ------- |
| float  | 4    | 单精度     |
| double | 8    | 双精度（常用） |

```java
double price = 19.99;
float height = 1.75F;  // F必须写
```

默认小数是 **double**

## 3️⃣ 字符类型

```java
char grade = 'A';
```

- 单引号

- 本质是一个 Unicode 数值

- 占 2 字节

## 4️⃣ 布尔类型

```java
boolean isPass = true;
```

只能是：

**true / false**

# 3. 引用数据类型

除 8 种基本类型外，全部都是引用类型。

常见：

- String

- 数组

- 类

- 接口

- 枚举

## 1️⃣ String

```java
String name = "Alice";
```

说明：

- String 是类

- 属于引用类型

- 底层存储在堆内存

## 2️⃣ 数组

```java
int[] numbers = {1, 2, 3};
```

## 3️⃣枚举

Java 的枚举本质是一个特殊的类。

```java
public enum Status {
    SUCCESS,
    FAILED,
    PENDING
}

// 使用
Status s = Status.SUCCESS;
```

### 枚举本质

枚举底层：

- 自动继承 `java.lang.Enum`

- 每个枚举值是一个 `public static final` 实例

- 线程安全

# 4. 常量

在 Java 中，常量通过 `final` 关键字声明。

```java
修饰符 final 数据类型 常量名 = 值;
```

示例：

```java
public static final int MAX_SIZE = 100;
```

为什么通常写成 `public static final`

| 关键字    | 含义    |
| ------ | ----- |
| public | 全局可访问 |
| static | 属于类   |
| final  | 不可修改  |

常量命名规范：**全大写 + 下划线**


