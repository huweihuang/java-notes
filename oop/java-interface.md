## 📖 前言

本教程旨在通过一个连贯的**图形面积计算系统**案例，深入浅出地讲解 Java 面向对象编程（OOP）的四大基石：**封装、继承、多态**以及**接口**。我们将模拟一个场景：系统需要支持多种图形（圆形、矩形、三角形），并能统一计算它们的面积和周长。

---

## 1. 封装 (Encapsulation)

**核心思想**：隐藏对象的内部细节，只暴露必要的操作接口。保护数据不被随意修改，提高安全性。

### 关键点

- 使用 `private` 修饰成员变量。
- 提供 `public` 的 `getter` 和 `setter` 方法来访问和修改数据。
- 可以在 setter 方法中加入逻辑校验。

### 代码示例

我们定义一个基础图形类 `Shape`，将属性私有化。

```java
public class Shape {
    // 1. 私有属性：外部无法直接访问
    private String color;
    private String name;

    // 2. 构造方法
    public Shape(String name, String color) {
        this.name = name;
        this.color = color;
    }

    // 3. 公开 Getter/Setter 方法
    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        // 可以在这里添加逻辑，例如禁止设置为空
        if (color != null && !color.isEmpty()) {
            this.color = color;
        }
    }

    public String getName() {
        return name;
    }

    // 定义一个计算面积的方法（留给子类去实现具体逻辑）
    public double getArea() {
        return 0.0;
    }
}
```

---

## 2. 继承 (Inheritance)

**核心思想**：代码复用。子类可以继承父类的属性和方法，并可以扩展新的功能或重写父类的方法。

### 关键点

- 使用 `extends` 关键字。
- 子类拥有父类非私有的所有成员。
- 使用 `@Override` 注解重写父类方法。
- 使用 `super` 关键字调用父类构造器或方法。

### 代码示例

创建 `Circle` (圆形) 和 `Rectangle` (矩形) 继承自 `Shape`。

```java
// 圆形类继承 Shape
public class Circle extends Shape {
    private double radius; // 半径

    public Circle(String color, double radius) {
        // 调用父类构造器
        super("圆形", color);
        this.radius = radius;
    }

    @Override
    public double getArea() {
        // 重写计算面积逻辑：π * r * r
        return Math.PI * radius * radius;
    }

    public double getRadius() {
        return radius;
    }
}

// 矩形类继承 Shape
public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(String color, double width, double height) {
        super("矩形", color);
        this.width = width;
        this.height = height;
    }

    @Override
    public double getArea() {
        // 重写计算面积逻辑：宽 * 高
        return width * height;
    }
}
```

---

## 3. 接口 (Interface)

**核心思想**：定义行为规范（契约）。接口只规定“能做什么”，不规定“怎么做”。一个类可以实现多个接口。

### 关键点

- 使用 `interface` 定义，使用 `implements` 实现。
- 接口中的方法默认是 `public abstract` (Java 8+ 可以有 default/static 方法)。
- 解决单继承的局限性，实现“多重继承”的效果。

### 代码示例

定义一个 `Drawable` 接口，规定所有能绘制的图形必须有 `draw()` 方法。

编辑

```java
// 定义接口
public interface Drawable {
    // 接口方法默认是 public abstract
    void draw();

    // Java 8+ 允许默认方法
    default void printInfo() {
        System.out.println("这是一个可绘制的图形");
    }
}

// 让 Circle 实现 Drawable 接口
// 注意：一个类可以同时 extends 一个类 并 implements 多个接口
public class Circle extends Shape implements Drawable {
    // ... (前面的代码保持不变)

    @Override
    public void draw() {
        System.out.println("正在绘制一个颜色为 " + getColor() + " 的圆形，半径：" + radius);
    }
}

// 让 Rectangle 也实现 Drawable 接口
public class Rectangle extends Shape implements Drawable {
    // ... (前面的代码保持不变)

    @Override
    public void draw() {
        System.out.println("正在绘制一个颜色为 " + getColor() + " 的矩形，宽：" + width + ", 高：" + height);
    }
}
```

---

## 4. 多态 (Polymorphism)

**核心思想**：同一个行为具有多个不同表现形式。

- **编译时多态**：方法重载 (Overload)。
- **运行时多态**：方法重写 (Override) + 父类引用指向子类对象。

### 关键点

- **向上转型**：`Shape s = new Circle(...)`。
- **动态绑定**：调用 `s.getArea()` 时，JVM 会在运行时判断 `s` 实际指向的是 `Circle` 还是 `Rectangle`，从而调用对应的方法。
- **向下转型**：如果需要调用子类特有方法，需强制类型转换 `(Circle) s`。

### 综合演示代码

这是本教程的核心，展示如何将上述概念结合起来使用。

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        // 1. 创建对象列表，使用父类类型作为泛型 (体现多态)
        List<Shape> shapes = new ArrayList<>();

        // 2. 添加不同类型的子类对象 (向上转型)
        shapes.add(new Circle("红色", 5.0));
        shapes.add(new Rectangle("蓝色", 4.0, 6.0));
        shapes.add(new Circle("绿色", 3.0));

        System.out.println("=== 开始处理图形列表 ===");

        // 3. 遍历列表，体现多态性
        for (Shape shape : shapes) {
            // 即使变量类型是 Shape，调用的也是具体子类的 getArea() 方法
            System.out.println("图形名称: " + shape.getName());
            System.out.println("面积: " + String.format("%.2f", shape.getArea()));

            // 4. 结合接口：判断是否可绘制并执行
            if (shape instanceof Drawable) {
                // 向下转型 (虽然这里可以直接强转，因为我们在上面确认了实现了接口)
                // 但更优雅的方式是直接利用多态，如果列表定义为 List<Drawable & Shape> 会更复杂
                // 这里为了演示，我们假设已知它们都实现了 Drawable
                ((Drawable) shape).draw(); 
            }

            System.out.println("-------------------------");
        }

        // 5. 单独演示接口默认方法
        Shape s = new Circle("黄色", 2.0);
        if(s instanceof Drawable) {
            ((Drawable) s).printInfo(); // 调用接口默认方法
        }
    }
}
```

### 运行结果预期

```shell
=== 开始处理图形列表 ===
图形名称: 圆形
面积: 78.54
正在绘制一个颜色为 红色 的圆形，半径：5.0
-------------------------
图形名称: 矩形
面积: 24.00
正在绘制一个颜色为 蓝色 的矩形，宽：4.0, 高：6.0
-------------------------
图形名称: 圆形
面积: 28.27
正在绘制一个颜色为 绿色 的圆形，半径：3.0
-------------------------
这是一个可绘制的图形
```

---

## 💡 总结与核心区别

表格

| 概念     | 关键词                        | 作用             | 生活类比                                           |
| ------ | -------------------------- | -------------- | ---------------------------------------------- |
| **封装** | `private`, `getter/setter` | 保护数据，隐藏实现细节    | 自动变速箱：你只需要踩油门，不需要知道齿轮怎么咬合。                     |
| **继承** | `extends`, `super`         | 代码复用，建立层级关系    | 儿子继承父亲的姓氏和房产，但可以有自己的职业。                        |
| **接口** | `interface`, `implements`  | 定义规范，解耦，实现多重能力 | 电器插头标准：只要符合国标插座，电视、冰箱都能插。                      |
| **多态** | 父类引用指向子类对象                 | 提高扩展性，统一调用     | “交通工具”：无论是汽车、飞机还是轮船，都可以执行 `.move()`，但跑起来的样子不同。 |

### 为什么这样设计？

在这个案例中，如果我们未来需要增加一个 `Triangle` (三角形)：

1. 只需新建一个类继承 `Shape` 并实现 `Drawable`。
2. **不需要修改** `Main` 方法中的任何循环逻辑。
3. 系统自动就能计算三角形面积并绘制它。

这就是面向对象编程带来的**高内聚、低耦合**和**易扩展性**。
