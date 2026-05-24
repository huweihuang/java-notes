# 1. 什么是类（Class）

类是：

> 对现实事物的抽象描述（属性 + 行为）

- 属性 → 成员变量

- 行为 → 方法

# 2. 类的声明

## 1️⃣ 基本语法

```java
访问修饰符 class 类名 {  
 // 成员变量  
 // 构造方法  
 // 成员方法  
}
```

示例：

```java
public class User {  
 String name;  
 int age;  
}
```

## 2️⃣ 命名规范

- 类名使用 **大驼峰**

- 一个 Java 文件通常一个 public 类

- 文件名必须与类名一致

# 3. 成员变量（属性）

定义在类中、方法外。

```java
public class User {  
 String name;  
 int age;  
}
```

特点：

- 有默认值

- 生命周期随对象

默认值：

| 类型      | 默认值   |
| ------- | ----- |
| int     | 0     |
| double  | 0.0   |
| boolean | false |
| 引用类型    | null  |

# 4. 对象的创建与初始化

## 1️⃣ 创建对象

```java
类名 对象名 = new 类名();

示例：

User user = new User();
```

执行过程：

1. 在堆内存中分配空间  
2. 成员变量赋默认值  
3. 执行构造方法  
4. 返回对象引用

# 5. 构造方法（Constructor）

构造方法用于：

> 初始化对象

## 1️⃣ 特点

- 方法名必须与类名相同

- 没有返回值（连 void 都不能写）

- 可重载

## 2️⃣ 默认构造方法

如果不写，系统自动生成：

```java
public User() {  
}
```

## 3️⃣ 自定义构造方法

```java
public class User {  

    String name;  
    int age;  

    public User(String name, int age) {  
        this.name = name;  
        this.age = age;  
    }  

}
```

使用：

```java
User user = new User("Tom", 18);
```

## 4️⃣ 构造方法重载

```java
public User() {  
}

public User(String name) {  
 this.name = name;  
}
```

# 6. this 关键字

表示当前对象。

用于：

- 区分成员变量与局部变量

- 调用本类构造方法

示例：

```java
this.name = name;
```

# 7. 方法

## 7.1.  方法的定义

方法是类的行为。

### 1️⃣ 基本语法

```java
访问修饰符 返回值类型 方法名(参数列表) {  
 方法体  
}
```

### 2️⃣ 示例

```java
public class Calculator {

    public int add(int a, int b) {
        return a + b;
    }
}
```

## 7.2. 方法调用

### 1️⃣ 实例方法调用

```java
Calculator c = new Calculator();  
int result = c.add(3, 5);
```

### 2️⃣ 静态方法调用

```java
public class MathUtil {

    public static int square(int x) {
        return x * x;
    }
}
```

调用：

```java
int result = MathUtil.square(5);
```

特点：

- 不需要创建对象

- 属于类

## 7.3. 方法参数

Java 参数传递是：

> 值传递

### 1️⃣ 基本类型

```java
public void change(int x) {  
 x = 100;  
}
```

外部变量不变。

### 2️⃣ 引用类型

传递的是地址的副本。

```java
public void change(User u) {  
 u.name = "Jerry";  
}
```

会修改对象内容。

## 7.4. 方法返回值

### 1️⃣ 有返回值

```java
public int sum(int a, int b) {  
 return a + b;  
}
```

### 2️⃣ 无返回值

```java
public void print(String msg) {  
 System.out.println(msg);  
}
```

## 7.5. 方法重载（Overload）

同名方法，参数不同。

```java
public int add(int a, int b)  
public double add(double a, double b)
```

判断标准：

- 参数类型

- 参数个数

- 参数顺序

⚠ 返回值不同不构成重载。

# 8. 访问修饰符

| 修饰符       | 同类  | 同包  | 子类  | 其他  | 类比GO     |
| --------- | --- | --- | --- | --- | -------- |
| public    | ✔   | ✔   | ✔   | ✔   | 方法名首字母大写 |
| protected | ✔   | ✔   | ✔   | ✖   |          |
| 默认        | ✔   | ✔   | ✖   | ✖   |          |
| private   | ✔   | ✖   | ✖   | ✖   | 方法名首字母小写 |

# 9. 类的完整示例

```java
public class Person {
    // 类的成员变量
    private String name;
    private int age;
    // 默认构造方法
    public Person() {
    }
    // 自定义构造方法
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    // 类的方法
    public void introduce() {
        System.out.println("我是" + name + "，年龄" + age);
    }
    // 类的方法
    public int getAge() {
        return age;
    }
}
```

使用：

```java
public class Main {
    // main函数
    public static void main(String[] args) {
        // 实例化类
        Person p = new Person("Alice", 20);
        p.introduce();
        // 方法调用
        int age = p.getAge();
        System.out.println(age);
    }
}
```

# 10. 类的初始化顺序（进阶理解）

对象创建时执行顺序：

1. 静态变量  
2. 静态代码块  
3. 成员变量  
4. 构造代码块  
5. 构造方法
