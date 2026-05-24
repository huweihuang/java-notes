# 1. 流程控制整体结构

Java 流程控制分三大类：

1️⃣ 顺序结构  
2️⃣ 分支结构（条件判断）  
3️⃣ 循环结构  
4️⃣ 跳转控制

```java
流程控制
 ├── 顺序
 ├── 分支
 │     ├── if
 │     ├── if-else
 │     └── switch
 ├── 循环
 │     ├── for
 │     ├── while
 │     ├── do-while
 │     └── for-each
 └── 跳转
       ├── break
       ├── continue
       └── return
```

# 2. 顺序结构

默认执行方式：

```java
int a = 10;  
int b = 20;  
int sum = a + b;  
System.out.println(sum);
```

自上而下依次执行。

# 3. 分支结构（条件控制）

## 1️⃣ if 语句

基本语法

```java
if (条件表达式) {  
 执行代码  
}
```

示例：

```java
int age = 18;  
if (age >= 18) {  
 System.out.println("成年人");  
}
```

## 2️⃣ if-else

```java
if (条件) {
    代码块1
} else {
    代码块2
}
```

示例：

```java
int score = 60;
if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}
```

## 3️⃣ if-else if-else

多条件判断：

```java
if (score >= 90) {  
 System.out.println("A");  
} else if (score >= 80) {  
 System.out.println("B");  
} else if (score >= 70) {  
 System.out.println("C");  
} else {  
 System.out.println("D");  
}
```

⚠ 执行顺序：从上到下匹配，第一个满足即停止。

## 4️⃣ switch 语句

用于多值分支判断。

基本语法（Java 8 传统写法）

```java
switch (表达式) {  
 case 值1:  
 代码;  
 break;  
 case 值2:  
 代码;  
 break;  
 default:  
 代码;  
}
```

示例：

```java
int day = 1;

switch (day) {  
 case 1:  
 System.out.println("周一");  
 break;  
 case 2:  
 System.out.println("周二");  
 break;  
 default:  
 System.out.println("其他");  
}
```

⚠ 如果不写 break 会发生“贯穿”（fall-through）。

### switch 新语法（Java 14+ 推荐）

```java
int day = 1;

switch (day) {  
 case 1 -> System.out.println("周一");  
 case 2 -> System.out.println("周二");  
 default -> System.out.println("其他");  
}
```

特点：

- 无需 break

- 更安全

- 可返回值

# 4. 循环结构

## 1️⃣ for 循环

最常用循环结构。

基本语法

```java
for (初始化; 条件; 更新) {  
 循环体  
}
```

示例：

```java
for (int i = 0; i < 5; i++) {  
 System.out.println(i);  
}
```

执行顺序：

初始化 → 条件判断 → 执行 → 更新 → 条件判断



## 2️⃣ while 循环

适用于不确定循环次数。

```java
while (条件) {  
 循环体  
}
```

示例：

```java
int i = 0;  
while (i < 5) {  
 System.out.println(i);  
 i++;  
}
```

## 3️⃣ do-while 循环

至少执行一次。

```java
do {  
 循环体  
} while (条件);
```

示例：

```java
int i = 0;  
do {  
 System.out.println(i);  
 i++;  
} while (i < 5);
```

区别：

| 类型       | 是否先判断 |
| -------- | ----- |
| while    | 是     |
| do-while | 否     |

## 4️⃣ 增强 for（for-each）

用于遍历数组或集合。

```java
int[] arr = {1, 2, 3};

for (int num : arr) {  
 System.out.println(num);  
}
```

特点：

- 简洁

- 不能修改数组结构

- 无索引

# 5. 跳转控制语句

## 1️⃣ break

作用：

- 跳出当前循环

- 结束 switch

```java
for (int i = 0; i < 10; i++) {  
 if (i == 5) {  
 break;  
 }  
}
```

## 2️⃣ continue

跳过当前循环本次执行，进入下一次。

```java
for (int i = 0; i < 5; i++) {  
 if (i == 2) {  
 continue;  
 }  
 System.out.println(i);  
}
```

## 3️⃣ return

结束整个方法。

```java
public void test(int x) {  
 if (x < 0) {  
 return;  
 }  
 System.out.println("继续执行");  
}
```
