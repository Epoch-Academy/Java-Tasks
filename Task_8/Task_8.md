# Task 8：JVM 基础与 Java 测试

## 1. 任务说明

本任务引入 JVM 基础和 Java 测试基础，重点包括 JVM、JRE、JDK 的关系，Java 程序运行过程，运行时内存区域，堆内存、栈内存、方法区、程序计数器、本地方法栈，垃圾回收基础，类加载机制，以及使用 JUnit 进行单元测试。

前面的任务主要关注如何编写 Java 程序。本任务开始关注两个更深一层的问题：

1. Java 程序运行时，内存中发生了什么。
2. 如何通过测试验证代码是否符合预期。

JVM 基础能帮助理解对象创建、方法调用、递归、栈溢出、内存溢出、`static` 初始化、类加载和垃圾回收。测试基础能帮助在程序变复杂后，用更稳定的方式验证代码，而不是只依赖手动运行和观察控制台输出。

## 2. 知识范围

本任务包含以下内容：

1. JVM、JRE、JDK 的关系。
2. Java 程序运行过程。
3. `.java`、`.class`、字节码。
4. JVM 运行时内存区域。
5. 程序计数器。
6. Java 虚拟机栈。
7. 栈帧。
8. 本地方法栈。
9. 堆内存。
10. 方法区和元空间。
11. 对象创建过程。
12. 引用变量与对象。
13. 垃圾回收基础。
14. 常见 JVM 错误。
15. 类加载过程。
16. 类加载器。
17. `static` 初始化顺序。
18. 测试的含义。
19. 单元测试。
20. JUnit 5。
21. 常见断言。
22. 异常测试。
23. 测试生命周期。
24. 参数化测试。
25. 可测试代码设计。

## 3. JVM、JRE、JDK

### 3.1 JVM

JVM 是 Java Virtual Machine 的缩写，表示 Java 虚拟机。

JVM 的作用是运行 Java 字节码。

Java 源代码不能直接被 JVM 运行，通常需要先编译为 `.class` 文件。

```text
Hello.java -> javac 编译 -> Hello.class -> JVM 运行
```

`.class` 文件中保存的是字节码。

### 3.2 JRE

JRE 是 Java Runtime Environment，表示 Java 运行环境。

JRE 包含：

1. JVM。
2. Java 程序运行所需的基础类库。

如果只是运行 Java 程序，理论上需要 JRE 即可。

### 3.3 JDK

JDK 是 Java Development Kit，表示 Java 开发工具包。

JDK 包含：

1. JRE。
2. 编译器 `javac`。
3. 运行工具 `java`。
4. 打包工具 `jar`。
5. 调试、分析等开发工具。

学习和开发 Java 程序通常安装 JDK。

### 3.4 三者关系

可以简单理解为：

```text
JDK 包含 JRE
JRE 包含 JVM
JVM 负责运行字节码
```

| 名称 | 作用 |
| ---- | ---- |
| JVM | 运行 Java 字节码 |
| JRE | 提供 Java 运行环境 |
| JDK | 提供 Java 开发工具 |

## 4. Java 程序运行过程

### 4.1 编译和运行

示例代码：

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello JVM");
    }
}
```

编译：

```bat
javac Hello.java
```

运行：

```bat
java Hello
```

执行过程：

1. 编写 `.java` 源文件。
2. 使用 `javac` 编译为 `.class` 字节码文件。
3. 使用 `java` 命令启动 JVM。
4. JVM 加载 `.class` 文件。
5. JVM 执行 `main` 方法。

### 4.2 字节码

字节码是 JVM 能识别的指令格式。

Java 的一个重要特点是“一次编译，到处运行”。这是因为 Java 源代码编译后生成的是平台无关的字节码，而不同系统上安装的 JVM 负责把字节码运行起来。

```text
同一份 .class 文件
-> Windows 上的 JVM
-> macOS 上的 JVM
-> Linux 上的 JVM
```

不同平台有不同 JVM，但它们都能识别 Java 字节码。

## 5. JVM 运行时内存区域

### 5.1 运行时内存区域概览

JVM 运行程序时，会把内存划分成不同区域。

基础阶段重点理解以下区域：

1. 程序计数器。
2. Java 虚拟机栈。
3. 本地方法栈。
4. 堆。
5. 方法区。

可以简单理解为：

```text
线程私有：程序计数器、Java 虚拟机栈、本地方法栈
线程共享：堆、方法区
```

线程私有表示每个线程都有自己的一份。线程共享表示多个线程共同使用。

### 5.2 程序计数器

程序计数器用于记录当前线程正在执行的字节码指令位置。

每个线程都有自己的程序计数器。

它的作用可以理解为：线程切换后，JVM 能知道这个线程下一步应该从哪里继续执行。

基础阶段只需要知道程序计数器很小，并且是线程私有的。

### 5.3 Java 虚拟机栈

Java 虚拟机栈用于支持 Java 方法调用。

每个线程都有自己的虚拟机栈。

每调用一个方法，JVM 会为这个方法创建一个栈帧。方法执行结束后，对应栈帧会出栈。

示例：

```java
public class StackDemo {
    public static void main(String[] args) {
        methodA();
    }

    public static void methodA() {
        methodB();
    }

    public static void methodB() {
        int number = 10;
        System.out.println(number);
    }
}
```

调用过程可以理解为：

```text
main 入栈
methodA 入栈
methodB 入栈
methodB 出栈
methodA 出栈
main 出栈
```

### 5.4 栈帧

栈帧是方法调用时在虚拟机栈中的一块内存。

栈帧中包含：

1. 局部变量表。
2. 操作数栈。
3. 方法返回地址。
4. 一些运行时辅助信息。

基础阶段重点理解局部变量表。

```java
public static int add(int a, int b) {
    int result = a + b;
    return result;
}
```

方法参数 `a`、`b` 和局部变量 `result` 都会和当前方法的栈帧相关。

### 5.5 本地方法栈

本地方法栈用于支持本地方法调用。

本地方法通常指使用 `native` 修饰的方法，它们可能由 C 或 C++ 等语言实现。

基础阶段不需要深入掌握本地方法栈，只需要知道它服务于本地方法。

## 6. 堆内存

### 6.1 什么是堆

堆用于存放对象实例。

使用 `new` 创建出来的对象，通常位于堆中。

```java
Student student = new Student("Alice", 18);
```

可以简单理解为：

1. `new Student(...)` 创建的对象在堆中。
2. `student` 是引用变量，位于当前方法的栈帧局部变量表中。
3. `student` 保存的是对象的引用。

### 6.2 引用变量和对象

示例：

```java
public class HeapDemo {
    public static void main(String[] args) {
        Student a = new Student("Alice", 18);
        Student b = a;

        b.setAge(20);

        System.out.println(a.getAge());
    }
}
```

`a` 和 `b` 是两个引用变量，但它们指向同一个堆中对象。因此通过 `b` 修改对象内容后，通过 `a` 也能看到变化。

这也是前面学习引用类型变量时容易混淆的重点。

### 6.3 堆和栈的区别

| 对比项 | 栈 | 堆 |
| ------ | -- | -- |
| 存放内容 | 方法调用、局部变量等 | 对象实例 |
| 是否线程共享 | 每个线程私有 | 多线程共享 |
| 生命周期 | 方法调用结束后栈帧出栈 | 对象不再被引用后等待 GC |
| 常见问题 | 栈溢出 | 内存溢出 |

注意：这里是基础阶段的简化理解，真实 JVM 实现会有更多细节。

## 7. 方法区和元空间

### 7.1 方法区

方法区用于保存类相关信息。

例如：

1. 类名。
2. 字段信息。
3. 方法信息。
4. 常量。
5. 静态变量相关信息。

方法区是 JVM 规范中的概念。

### 7.2 元空间

在现代 HotSpot JVM 中，方法区的实现主要是元空间。

元空间使用的是本地内存，而不是普通 Java 堆内存。

基础阶段可以这样理解：

1. 方法区是规范概念。
2. 元空间是 HotSpot JVM 对方法区的一种实现。
3. 类的元数据信息通常存放在元空间中。

### 7.3 字符串常量池回顾

前面学习过字符串常量池。

```java
String a = "Java";
String b = "Java";
```

`a` 和 `b` 指向同一个字符串常量对象。

字符串常量池与 JVM 内存有关。基础阶段只需要知道：字符串字面量会被 Java 特殊管理，以便复用相同字符串内容。

## 8. 对象创建过程

### 8.1 对象创建的大致步骤

当执行：

```java
Student student = new Student("Alice", 18);
```

JVM 大致会做这些事情：

1. 检查 `Student` 类是否已经加载。
2. 如果没有加载，先进行类加载。
3. 在堆中为对象分配内存。
4. 为对象字段设置默认值。
5. 执行构造方法中的初始化逻辑。
6. 将对象引用赋值给变量 `student`。

### 8.2 默认值和构造方法

对象创建时，成员变量会先有默认值，再执行构造方法。

```java
public class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

默认值：

1. `name` 默认是 `null`。
2. `age` 默认是 `0`。

构造方法执行后：

1. `name` 变为传入的姓名。
2. `age` 变为传入的年龄。

## 9. 垃圾回收基础

### 9.1 什么是垃圾回收

垃圾回收简称 GC，Garbage Collection。

GC 用于自动回收不再使用的对象所占用的内存。

Java 程序员通常不需要手动释放对象内存。

```java
public class GcIntroDemo {
    public static void main(String[] args) {
        Student student = new Student("Alice", 18);
        student = null;
    }
}
```

当对象不再被任何有效引用指向时，就可能成为垃圾回收的对象。

### 9.2 可达性分析

现代 JVM 通常通过可达性分析判断对象是否还被使用。

可以简单理解为：

1. 从一组根对象出发。
2. 沿着引用关系查找对象。
3. 能找到的对象是可达的。
4. 找不到的对象可能被回收。

基础阶段不要求掌握所有 GC Roots，只需要知道对象不是简单因为“出了作用域”立刻消失，而是等待 JVM 判断和回收。

### 9.3 `System.gc`

`System.gc()` 可以建议 JVM 进行垃圾回收。

```java
System.gc();
```

注意：它只是建议，不保证立即执行。

实际开发中通常不依赖手动调用 `System.gc()`。

## 10. 常见 JVM 错误

### 10.1 栈溢出

递归没有正确出口时，可能导致栈溢出。

```java
public class StackOverflowDemo {
    public static void call() {
        call();
    }

    public static void main(String[] args) {
        call();
    }
}
```

常见错误：

```text
StackOverflowError
```

原因是方法不断调用自己，栈帧不断入栈，最终超过虚拟机栈容量。

### 10.2 堆内存溢出

如果不断创建对象，并且这些对象一直被引用，可能导致堆内存溢出。

```java
import java.util.ArrayList;

public class OutOfMemoryDemo {
    public static void main(String[] args) {
        ArrayList<byte[]> list = new ArrayList<>();

        while (true) {
            list.add(new byte[1024 * 1024]);
        }
    }
}
```

常见错误：

```text
OutOfMemoryError
```

运行这种代码可能占用大量内存。学习时只需要理解原因，不建议随意运行。

### 10.3 内存泄漏

内存泄漏是指对象已经不再需要，但仍然被引用，导致 GC 无法回收。

示例场景：

1. 静态集合长期保存无用对象。
2. 监听器注册后没有移除。
3. 缓存只增加不清理。
4. 线程长期持有大对象。

Java 有 GC，但仍然可能发生内存泄漏。

## 11. 类加载机制

### 11.1 什么是类加载

类加载是 JVM 把 `.class` 字节码加载到内存，并准备好运行的过程。

当程序第一次主动使用某个类时，JVM 可能会加载这个类。

例如：

```java
Student student = new Student("Alice", 18);
```

如果 `Student` 类还没有被加载，JVM 会先加载它。

### 11.2 类加载过程

类加载过程通常包括：

1. 加载。
2. 链接。
3. 初始化。

链接又包括：

1. 验证。
2. 准备。
3. 解析。

基础阶段重点理解：

1. 加载：把类的字节码读入 JVM。
2. 链接：检查和准备类信息。
3. 初始化：执行类初始化逻辑，例如静态变量赋值和静态代码块。

### 11.3 类加载器

类加载器用于加载类。

常见类加载器：

| 类加载器 | 作用 |
| -------- | ---- |
| Bootstrap ClassLoader | 加载 Java 核心类 |
| Platform ClassLoader | 加载平台相关类 |
| Application ClassLoader | 加载应用程序类路径下的类 |

示例：

```java
public class ClassLoaderDemo {
    public static void main(String[] args) {
        System.out.println(String.class.getClassLoader());
        System.out.println(ClassLoaderDemo.class.getClassLoader());
    }
}
```

`String` 是 Java 核心类，它的类加载器输出可能是 `null`，表示由 Bootstrap ClassLoader 加载。

### 11.4 `static` 初始化顺序

示例：

```java
public class StaticInitDemo {
    static int number = 10;

    static {
        System.out.println("静态代码块执行");
        number = 20;
    }

    public static void main(String[] args) {
        System.out.println(number);
    }
}
```

类初始化时，会按照代码出现顺序执行静态变量赋值和静态代码块。

基础阶段应记住：

1. 静态内容属于类。
2. 类初始化通常早于对象创建。
3. 静态变量和静态代码块按书写顺序执行。

## 12. 测试基础

### 12.1 什么是测试

测试用于验证程序是否符合预期。

手动测试通常是运行程序、输入数据、观察输出。

自动化测试是把验证逻辑写成代码，让测试工具自动执行。

测试的作用包括：

1. 验证代码正确性。
2. 防止修改代码时破坏已有功能。
3. 让代码行为更清晰。
4. 提高重构信心。

### 12.2 什么是单元测试

单元测试用于测试程序中较小的功能单元。

例如：

1. 一个工具方法。
2. 一个业务方法。
3. 一个类中的某个计算逻辑。

示例业务类：

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int divide(int a, int b) {
        if (b == 0) {
            throw new IllegalArgumentException("除数不能为 0");
        }

        return a / b;
    }
}
```

可以为 `add` 和 `divide` 分别编写测试。

## 13. JUnit 5

### 13.1 什么是 JUnit

JUnit 是 Java 中常用的单元测试框架。

JUnit 5 是当前常见版本。

如果使用 Maven 项目，可以在 `pom.xml` 中添加测试依赖。

```xml
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.2</version>
    <scope>test</scope>
</dependency>
```

如果使用 IDEA，也可以通过 IDEA 的提示添加 JUnit 依赖。

### 13.2 第一个测试

测试类通常放在 `src/test/java` 目录下。

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorTest {
    @Test
    void addShouldReturnSum() {
        Calculator calculator = new Calculator();

        int result = calculator.add(10, 20);

        assertEquals(30, result);
    }
}
```

其中：

1. `@Test` 表示这是一个测试方法。
2. `assertEquals(30, result)` 表示期望结果是 `30`。
3. 如果实际结果不是 `30`，测试失败。

### 13.3 测试命名

测试方法名应表达测试意图。

示例：

```java
void addShouldReturnSum()
void divideShouldReturnResult()
void divideShouldThrowExceptionWhenDivisorIsZero()
```

好的测试名能让人快速理解测试目标。

## 14. 常见断言

### 14.1 基础断言

JUnit 常见断言：

| 断言 | 含义 |
| ---- | ---- |
| `assertEquals(expected, actual)` | 判断是否相等 |
| `assertNotEquals(unexpected, actual)` | 判断是否不相等 |
| `assertTrue(condition)` | 判断是否为 `true` |
| `assertFalse(condition)` | 判断是否为 `false` |
| `assertNull(value)` | 判断是否为 `null` |
| `assertNotNull(value)` | 判断是否不为 `null` |
| `assertThrows(type, executable)` | 判断是否抛出指定异常 |

示例：

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertNotNull;
import static org.junit.jupiter.api.Assertions.assertTrue;

public class AssertionDemoTest {
    @Test
    void basicAssertions() {
        String text = "Java";

        assertNotNull(text);
        assertTrue(text.startsWith("J"));
        assertFalse(text.isEmpty());
    }
}
```

### 14.2 异常测试

```java
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class CalculatorExceptionTest {
    @Test
    void divideShouldThrowExceptionWhenDivisorIsZero() {
        Calculator calculator = new Calculator();

        IllegalArgumentException exception = assertThrows(
                IllegalArgumentException.class,
                () -> calculator.divide(10, 0)
        );

        assertEquals("除数不能为 0", exception.getMessage());
    }
}
```

`assertThrows` 可以验证某段代码是否抛出指定类型异常。

## 15. 测试生命周期

### 15.1 常见生命周期注解

JUnit 5 常见生命周期注解：

| 注解 | 含义 |
| ---- | ---- |
| `@BeforeEach` | 每个测试方法执行前运行 |
| `@AfterEach` | 每个测试方法执行后运行 |
| `@BeforeAll` | 所有测试方法执行前运行一次 |
| `@AfterAll` | 所有测试方法执行后运行一次 |

示例：

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorLifecycleTest {
    private Calculator calculator;

    @BeforeEach
    void setUp() {
        calculator = new Calculator();
    }

    @Test
    void addShouldReturnSum() {
        assertEquals(30, calculator.add(10, 20));
    }

    @Test
    void divideShouldReturnResult() {
        assertEquals(5, calculator.divide(10, 2));
    }
}
```

`@BeforeEach` 适合准备每个测试都需要的对象。

## 16. 参数化测试

### 16.1 为什么需要参数化测试

如果同一个方法需要使用多组数据测试，可以使用参数化测试。

普通写法可能需要写多个测试方法。参数化测试可以让一份测试逻辑运行多次。

### 16.2 `@ParameterizedTest`

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;

public class CalculatorParameterizedTest {
    @ParameterizedTest
    @CsvSource({
            "1, 2, 3",
            "10, 20, 30",
            "-1, 1, 0"
    })
    void addShouldReturnSum(int a, int b, int expected) {
        Calculator calculator = new Calculator();

        assertEquals(expected, calculator.add(a, b));
    }
}
```

其中：

1. `@ParameterizedTest` 表示参数化测试。
2. `@CsvSource` 提供多组测试数据。
3. 测试方法会根据数据运行多次。

## 17. 可测试代码设计

### 17.1 可测试代码的特点

容易测试的代码通常具有以下特点：

1. 方法职责清晰。
2. 输入和输出明确。
3. 依赖较少。
4. 不把大量逻辑都写在 `main` 方法中。
5. 业务逻辑和输入输出分离。

不易测试的代码：

```java
public class BadDemo {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        System.out.println(a + b);
    }
}
```

更易测试的代码：

```java
public class AddService {
    public int add(int a, int b) {
        return a + b;
    }
}
```

### 17.2 测试边界情况

测试不仅要测正常情况，也要测边界情况。

例如成绩等级方法：

```java
public class ScoreService {
    public String getLevel(int score) {
        if (score < 0 || score > 100) {
            throw new IllegalArgumentException("成绩不合法");
        }

        if (score >= 90) {
            return "优秀";
        }

        if (score >= 80) {
            return "良好";
        }

        if (score >= 60) {
            return "及格";
        }

        return "不及格";
    }
}
```

应测试：

1. `90`。
2. `89`。
3. `80`。
4. `79`。
5. `60`。
6. `59`。
7. `-1`。
8. `101`。

边界值经常最容易出错。

## 18. 代码示例

在 Maven 或已添加 JUnit 5 的 IDEA 项目中创建以下文件。

### 18.1 `MemoryStudent.java`

```java
public class MemoryStudent {
    private String name;
    private int age;

    public MemoryStudent(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void grow() {
        age++;
    }

    public int getAge() {
        return age;
    }
}
```

### 18.2 `MemoryDemo.java`

```java
public class MemoryDemo {
    public static void main(String[] args) {
        MemoryStudent a = new MemoryStudent("Alice", 18);
        MemoryStudent b = a;

        b.grow();

        System.out.println(a.getAge());
    }
}
```

运行后应重点观察：`a` 和 `b` 指向同一个堆中对象。

### 18.3 `ScoreService.java`

```java
public class ScoreService {
    public String getLevel(int score) {
        if (score < 0 || score > 100) {
            throw new IllegalArgumentException("成绩不合法");
        }

        if (score >= 90) {
            return "优秀";
        }

        if (score >= 80) {
            return "良好";
        }

        if (score >= 60) {
            return "及格";
        }

        return "不及格";
    }
}
```

### 18.4 `ScoreServiceTest.java`

```java
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.CsvSource;

import static org.junit.jupiter.api.Assertions.assertEquals;
import static org.junit.jupiter.api.Assertions.assertThrows;

public class ScoreServiceTest {
    private final ScoreService scoreService = new ScoreService();

    @ParameterizedTest
    @CsvSource({
            "100, 优秀",
            "90, 优秀",
            "89, 良好",
            "80, 良好",
            "79, 及格",
            "60, 及格",
            "59, 不及格",
            "0, 不及格"
    })
    void getLevelShouldReturnCorrectLevel(int score, String expected) {
        assertEquals(expected, scoreService.getLevel(score));
    }

    @ParameterizedTest
    @CsvSource({
            "-1",
            "101"
    })
    void getLevelShouldThrowExceptionWhenScoreInvalid(int score) {
        assertThrows(IllegalArgumentException.class, () -> scoreService.getLevel(score));
    }
}
```

运行后应重点观察：

1. 参数化测试如何用多组数据测试同一个方法。
2. 边界值如何覆盖等级判断。
3. `assertThrows` 如何测试异常情况。

## 19. 练习

### 19.1 JVM 基础概念练习

创建 `JvmNote.md`，完成以下要求：

1. 说明 JVM、JRE、JDK 的关系。
2. 说明 `.java`、`.class`、字节码的关系。
3. 说明 Java 程序从编译到运行的大致过程。
4. 列出 JVM 运行时内存区域。
5. 说明哪些区域线程私有，哪些区域线程共享。
6. 说明堆和栈的主要区别。
7. 说明方法区和元空间的关系。

### 19.2 堆栈理解练习

创建 `HeapStackPractice.java`，完成以下要求：

1. 定义 `Person` 类，包含 `name` 和 `age`。
2. 创建 `Person p1 = new Person("Alice", 18);`。
3. 创建 `Person p2 = p1;`。
4. 通过 `p2` 修改年龄。
5. 通过 `p1` 输出年龄，观察结果。
6. 写注释说明对象在堆中，引用变量与当前方法栈帧相关。
7. 编写三个方法互相调用，观察调用顺序。

### 19.3 JVM 错误练习

创建 `JvmErrorNote.md`，完成以下要求：

1. 说明什么是 `StackOverflowError`。
2. 写出递归没有出口导致栈溢出的示例代码。
3. 说明什么是 `OutOfMemoryError`。
4. 说明为什么持续创建并保存大对象可能导致堆内存溢出。
5. 说明什么是内存泄漏。
6. 列举至少 3 种可能导致内存泄漏的场景。

### 19.4 类加载练习

创建 `ClassLoadingPractice.java`，完成以下要求：

1. 定义类中的静态变量。
2. 定义静态代码块。
3. 定义构造方法。
4. 在 `main` 方法中创建对象。
5. 输出观察静态代码块和构造方法的执行顺序。
6. 输出当前类的类加载器。
7. 输出 `String.class.getClassLoader()` 的结果。
8. 写注释说明类初始化和对象创建的区别。

### 19.5 JUnit 基础练习

创建 `Calculator.java` 和 `CalculatorTest.java`，完成以下要求：

1. `Calculator` 中定义 `add`、`subtract`、`multiply`、`divide` 方法。
2. `divide` 遇到除数为 `0` 时抛出 `IllegalArgumentException`。
3. 使用 `@Test` 测试 `add` 方法。
4. 使用 `assertEquals` 测试计算结果。
5. 使用 `assertThrows` 测试除数为 `0` 的情况。
6. 使用 `@BeforeEach` 创建测试对象。

### 19.6 参数化测试练习

创建 `ScoreService.java` 和 `ScoreServiceTest.java`，完成以下要求：

1. `ScoreService` 中定义 `getLevel(int score)`。
2. 根据成绩返回 `优秀`、`良好`、`及格`、`不及格`。
3. 成绩小于 `0` 或大于 `100` 时抛出异常。
4. 使用 `@ParameterizedTest` 测试多个正常成绩。
5. 使用 `@CsvSource` 提供测试数据。
6. 使用 `assertThrows` 测试非法成绩。
7. 至少测试 `90`、`89`、`80`、`79`、`60`、`59`。

### 19.7 综合练习

创建 `Task8Practice.java`、`OrderService.java` 和 `OrderServiceTest.java`，完成以下要求：

1. 定义 `Order` 类，包含 `id`、`amount`、`status`。
2. 定义 `OrderStatus` 枚举，包含 `CREATED`、`PAID`、`CANCELED`。
3. `OrderService` 中定义 `pay(Order order)` 方法。
4. 当订单为 `CREATED` 时，支付后状态改为 `PAID`。
5. 当订单为 `CANCELED` 时，支付应抛出异常。
6. 使用注释说明 `Order` 对象和引用变量与堆、栈的关系。
7. 使用 JUnit 测试正常支付。
8. 使用 JUnit 测试取消订单不能支付。
9. 使用断言验证状态变化。
10. 在学习总结中说明 JVM 内存理解如何帮助分析对象状态变化。

## 20. 验收标准

满足以下条件视为完成 Task 8：

1. 能说明 JVM、JRE、JDK 的关系。
2. 能说明 Java 程序从 `.java` 到运行的大致过程。
3. 能说明字节码的作用。
4. 能列出 JVM 运行时内存区域。
5. 能说明程序计数器的基本作用。
6. 能说明 Java 虚拟机栈和栈帧的关系。
7. 能说明堆内存用于保存对象实例。
8. 能说明引用变量和对象之间的关系。
9. 能说明堆和栈的主要区别。
10. 能说明方法区和元空间的基本关系。
11. 能说明对象创建的大致过程。
12. 能说明垃圾回收的基本含义。
13. 能说明栈溢出和堆内存溢出的常见原因。
14. 能说明类加载过程的基本阶段。
15. 能说明类加载器的基本作用。
16. 能说明 `static` 初始化顺序。
17. 能说明测试和单元测试的作用。
18. 能使用 JUnit 5 编写基础测试。
19. 能使用常见断言验证结果。
20. 能使用 `assertThrows` 测试异常。
21. 能使用 `@BeforeEach` 准备测试对象。
22. 能使用参数化测试覆盖多组数据。
23. 能为边界值编写测试。
24. 能独立完成 `Task8Practice.java` 和相关测试类。

## 21. 提交规范

1. 学习/实验总结报告统一命名为 `Task_8_report.md`，放置在 `Task_8/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_8/src/` 目录下（或在 `Task_8/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_8/` 目录中的本阶段内容。
