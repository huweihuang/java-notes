# 1. 什么是集合？

**数组 (Array)** 是固定长度的容器，装满了就不能再加。  
**集合 (Collection)** 是**可变长度**的容器，想装多少装多少，专门用来存对象。

> **一句话总结**：集合就是 Java 提供的“超级工具箱”，帮你轻松管理一堆数据。

# 2. 核心家族图谱

Java 集合主要分为两大派系：**List** 和 **Map**（Set 通常作为 List 的补充）。

表格

| 接口       | 特点                                          | 核心实现类                       | 生活类比                        | 适用场景                        |
| -------- | ------------------------------------------- | --------------------------- | --------------------------- | --------------------------- |
| **List** | **有序**、**可重复**<br>(像排队)                     | `ArrayList`<br>`LinkedList` | **数组/清单**<br>(购物清单，可以买两个苹果) | 需要按顺序存取，或经常通过下标访问数据。        |
| **Set**  | **无序**、**不可重复**<br>(像抽奖箱)                   | `HashSet`<br>`TreeSet`      | **集邮册**<br>(同样的邮票只放一张)      | 需要去重，不关心顺序。                 |
| **Map**  | **键值对 (Key-Value)**<br>**Key 唯一**，Value 可重复 | `HashMap`<br>`TreeMap`      | **字典/通讯录**<br>(查“张三”得“电话”)  | 需要通过一个关键信息（如ID、姓名）快速查找对应数据。 |

# 3. 三大主力详解

## ArrayList (最常用)

- **底层**：动态数组。

- **优点**：查询快（知道下标瞬间找到），遍历快。

- **缺点**：增删慢（中间插队，后面的人都要挪位置）。

- **用法**：
  
  ```java
  import java.util.ArrayList;
  import java.util.List;
  
  List<String> list = new ArrayList<>();
  list.add("Java");      // 添加
  list.get(0);           // 获取 (快)
  list.remove("Java");   // 删除 (慢，需移动元素)
  ```

## HashMap (最常用)

- **底层**：哈希表 (数组 + 链表 + 红黑树)。

- **优点**：通过 Key 查找速度极快（几乎瞬间）。

- **注意**：Key 不能重复，如果 put 相同的 Key，新值会覆盖旧值。

- **用法**：
  
  ```java
  Map<String, Integer> map = new HashMap<>();
  map.put("年龄", 18);       // 存
  map.get("年龄");           // 取 (快)
  map.containsKey("年龄");   // 判断是否存在
  ```

## HashSet (去重神器)

- **底层**：其实就是个“只有 Key 没有 Value”的 HashMap。

- **优点**：自动去重。

- **用法**：
  
  ```java
  Set<String> set = new HashSet<>();
  set.add("A");
  set.add("A"); // 添加失败，集合里还是只有一个 "A"
  ```

*(注：`LinkedList` 适合频繁在头尾增删的场景，如队列；`TreeMap/TreeSet` 适合需要自动排序的场景。)*

# 4. 怎么遍历集合？ (通用招式)

无论 List、Set 还是 Map 的 Key/Set，都可以用这两种方式：

## 招式一：增强型 for 循环 (最简洁)

```java
// 遍历 List 或 Set
for (String item : list) {
    System.out.println(item);
}

// 遍历 Map (推荐方式)
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + " = " + entry.getValue());
}
```

## 招式二：Lambda + Stream (Java 8 现代写法)

```java
// 一行代码打印所有
list.forEach(System.out::println);

// 过滤出长度大于3的元素
list.stream()
    .filter(s -> s.length() > 3)
    .forEach(System.out::println);
```

# 5. 避坑指南

- **泛型别省**：一定要写 `<String>` 等类型，不要裸用 `List list`，否则取出来全是 `Object`，还要强转，容易报错。
- **判空习惯**：使用前最好判断 `if (list != null && !list.isEmpty())`，防止空指针异常。
- **线程安全**：`ArrayList` 和 `HashMap` **不是**线程安全的。如果在多线程环境下使用，请用 `CopyOnWriteArrayList` 或 `ConcurrentHashMap` (进阶内容)。
