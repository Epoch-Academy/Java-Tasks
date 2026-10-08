# Task 3：Java 面向对象基础

## 1. 任务说明

本任务引入 Java 面向对象编程的核心概念，重点包括类、方法、包、访问修饰符、构造方法、方法重载、封装、继承、多态、方法重写、抽象类、接口、内部类以及 `main` 方法参数。

Java 是一门面向对象编程语言，常简称为 OOP（Object-Oriented Programming）语言。面向对象编程强调使用对象描述现实或业务中的事物，并通过对象的属性和行为组织程序。

Java 面向对象通常具有三大特性：

1. 封装。
2. 继承。
3. 多态。

本任务中的概念较多，应重点理解各概念之间的关系，而不是只记忆语法形式。

## 2. 知识范围

本任务包含以下内容：

1. 类和方法。
2. 包。
3. 类的定义与类的修饰符。
4. 成员变量、成员方法与成员修饰符。
5. 方法修饰符。
6. `static` 静态修饰符。
7. 构造方法。
8. 方法重载。
9. 封装。
10. 继承。
11. 多态与方法重写。
12. 抽象类。
13. 接口。
14. 抽象类与接口的比较。
15. 内部类。
16. 命令行参数。

## 3. 什么是类和方法

### 3.1 从现实对象引入类

现实世界中存在大量对象，例如学生、老师、汽车、银行卡、订单。

以学生为例，学生通常具有一些数据：

| 对象 | 数据                   |
| ---- | ---------------------- |
| 学生 | 姓名、年龄、学号、成绩 |

学生也可以执行一些动作：

| 对象 | 动作                 |
| ---- | -------------------- |
| 学生 | 学习、考试、提交作业 |

在 Java 中，可以使用类描述一种对象。类用于说明这种对象具有什么数据，以及可以执行什么动作。

```text
学生类
数据：姓名、年龄、成绩
动作：学习、考试、计算等级
```

类不是某一个具体学生，而是对“学生这种对象”的抽象描述。根据类可以创建具体对象，例如张三这个学生、李四这个学生。

### 3.2 类的含义

类用于描述对象本身。一个类通常包含两部分内容：

1. 成员变量：描述对象具有什么数据。
2. 方法：描述对象可以执行什么动作。

示例：

```java
public class Student {
    String name;
    int age;

    void study() {
        System.out.println(name + " 正在学习");
    }
}
```

其中：

1. `Student` 是类名。
2. `name` 和 `age` 是成员变量。
3. `study` 是方法。

### 3.3 方法的含义

方法是写在类中的一段具有名称的代码块，用于表示对象可以执行的动作，或者表示某段可重复使用的逻辑。

```java
void study() {
    System.out.println("正在学习");
}
```

方法通常由方法声明和方法体组成。

```text
返回值类型 方法名(参数列表) {
    方法体;
}
```

示例：

```java
int add(int a, int b) {
    return a + b;
}
```

其中：

1. `int` 是返回值类型。
2. `add` 是方法名。
3. `int a, int b` 是参数列表。
4. `return a + b;` 表示返回方法执行结果。

方法名和参数类型共同构成方法签名。更准确地说，方法签名主要由方法名、参数类型和参数顺序决定，用于区分不同方法。参数名不属于方法签名，返回值类型也不属于方法签名。

```java
int add(int a, int b) {
    return a + b;
}
```

上述方法的方法名是 `add`，参数类型是 `(int, int)`。方法声明中的 `int a` 和 `int b` 称为形参，形参用于接收调用方法时传入的数据。

调用方法时传入的具体数据称为实参。

```java
int result = add(10, 20);
```

`10` 和 `20` 是实参，它们会传递给方法中的参数 `a` 和 `b`。

### 3.4 类、对象、方法的关系

类用于描述一类对象；对象是根据类创建出来的具体个体；方法表示对象可以执行的动作。

```java
Student student = new Student();
student.name = "LiMing";
student.study();
```

其中：

1. `new Student()` 创建了一个学生对象。
2. `student` 保存该对象的引用。
3. `student.study()` 调用该对象的 `study` 方法。

## 4. 什么是包

### 4.1 包的含义

包（package）用于组织 Java 类。随着项目中的类越来越多，如果所有类都放在同一层级，会导致命名冲突和管理混乱。

包的作用包括：

1. 对类进行分组管理。
2. 减少类名冲突。
3. 配合访问修饰符控制访问范围。

例如：

```text
com.example.school
com.example.bank
com.example.shop
```

这些包名可以分别存放学校、银行、商城相关类。

### 4.2 包的声明

包声明使用 `package` 关键字，通常写在 Java 文件第一行。

```java
package com.example.school;

public class Student {
}
```

如果类声明了包，文件目录通常也应与包名对应。

```text
com/example/school/Student.java
```

### 4.3 导入其他包中的类

使用其他包中的类时，可以使用 `import` 导入。

```java
import java.util.Scanner;

public class InputDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
    }
}
```

`java.lang` 包中的类会自动导入，例如 `String`、`System`。其他包中的类通常需要手动导入。

如果一个文件中同时存在 `package` 和 `import`，`package` 必须写在最前面，`import` 写在 `package` 之后、类定义之前。

```java
package com.example.demo;

import java.util.Scanner;

public class Demo {
}
```

## 5. 如何定义类

### 5.1 类定义语法

类定义的一般形式如下：

```text
修饰符 class 类名 {
    类体;
}
```

示例：

```java
public class Student {
}
```

其中：

1. `public` 是类的修饰符。
2. `class` 表示定义类。
3. `Student` 是类名。
4. `{}` 中是类体。

### 5.2 类名规范

类名通常使用大驼峰命名法，即每个单词首字母大写。

```java
public class Student {
}

public class BankAccount {
}

public class OrderItem {
}
```

如果一个 Java 文件中有 `public` 类，文件名必须与 `public` 类名完全一致。

```text
Student.java
```

### 5.3 一个文件中的多个类

一个 `.java` 文件中可以写多个类，但最多只能有一个 `public` 类。

```java
public class StudentDemo {
}

class Student {
}
```

上述代码应保存为：

```text
StudentDemo.java
```

一般来说，建议一个文件只写一个主要类，文件名与类名保持一致。

## 6. 类的修饰符

### 6.1 `public`

`public` 修饰的类可以被其他包中的代码访问。

```java
public class Student {
}
```

如果类需要作为外部可直接使用的类型，通常使用 `public` 修饰。

### 6.2 默认修饰符

类也可以不写访问修饰符。

```java
class Student {
}
```

这种类只能在同一个包中访问，称为默认访问权限。

### 6.3 `protected` 与 `private`

`protected` 和 `private` 也可以作为类的访问修饰符，但它们不能修饰直接写在 `.java` 文件中的顶层类。

```java
// private class Student {
// } // 错误：顶层类不能使用 private

// protected class Teacher {
// } // 错误：顶层类不能使用 protected
```

`protected` 和 `private` 可以修饰内部类。内部类会在第 20 部分单独讲解。

```java
public class Outer {
    private class Inner {
    }
}
```

类修饰符还包括 `final` 和 `abstract`。`final` 与继承关系有关，将在继承部分讲解；`abstract` 与抽象类有关，将在抽象类部分讲解。

## 7. 成员变量和方法

### 7.1 成员变量

成员变量定义在类中、方法外，用于保存对象的数据。

```java
public class Student {
    String name;
    int age;
    double score;
}
```

每个对象都有自己的一份成员变量。

```java
Student s1 = new Student();
s1.name = "LiMing";

Student s2 = new Student();
s2.name = "ZhangSan";

System.out.println(s1.name); // LiMing
System.out.println(s2.name); // ZhangSan
```

### 7.2 成员方法

成员方法定义在类中，用于描述对象的行为。

```java
public class Student {
    String name;

    void study() {
        System.out.println(name + " 正在学习");
    }
}
```

调用成员方法需要先创建对象。

```java
Student student = new Student();
student.name = "LiMing";
student.study();
```

### 7.3 成员变量与局部变量

成员变量定义在类中、方法外；局部变量定义在方法或代码块中。

```java
public class Student {
    String name; // 成员变量

    void study() {
        int minutes = 30; // 局部变量
        System.out.println(name + " 学习了 " + minutes + " 分钟");
    }
}
```

成员变量属于对象；局部变量只在定义它的方法或代码块中有效。

## 8. 成员的修饰符

### 8.1 访问修饰符

成员变量和方法可以使用访问修饰符控制访问范围。

| 修饰符      | 同一个类 | 同一个包 | 不同包子类 | 不同包非子类 |
| ----------- | -------- | -------- | ---------- | ------------ |
| `private`   | 可以     | 不可以   | 不可以     | 不可以       |
| 默认        | 可以     | 可以     | 不可以     | 不可以       |
| `protected` | 可以     | 可以     | 可以       | 不可以       |
| `public`    | 可以     | 可以     | 可以       | 可以         |

最基础的用法：

1. 成员变量使用 `private`。
2. 需要外部调用的方法使用 `public`。

其中，`protected` 对不同包子类的访问与继承关系有关，后续会在继承部分继续说明。

### 8.2 `static`

成员变量可以使用 `static` 修饰。`static` 的详细含义会在第 10 部分单独讲解。

### 8.3 `final`

`final` 修饰成员变量时，表示该变量只能赋值一次。

```java
public class Student {
    private final String id;

    public Student(String id) {
        this.id = id;
    }
}
```

上述代码中，`id` 在构造方法中完成赋值，之后不能再次修改。

## 9. 方法的修饰符

### 9.1 访问修饰符

方法可以使用 `private`、默认、`protected`、`public` 控制访问范围。

```java
public class Student {
    public void study() {
        System.out.println("学习");
    }

    private void checkScore() {
        System.out.println("检查成绩");
    }
}
```

`public` 方法通常表示对外提供的功能；`private` 方法通常只用于类内部辅助逻辑。

### 9.2 `static` 方法

方法可以使用 `static` 修饰。`main` 方法就是一个 `static` 方法。`static` 的详细含义会在第 10 部分单独讲解。

```java
public static void main(String[] args) {
}
```

### 9.3 `final` 方法

`final` 修饰的方法不能被子类重写。

```java
public class Animal {
    public final void breathe() {
        System.out.println("呼吸");
    }
}
```

### 9.4 `abstract` 方法

`abstract` 方法只有方法声明，没有方法体。

```java
public abstract void makeSound();
```

抽象方法必须定义在抽象类或接口中。

## 10. `static` 静态修饰符

### 10.1 什么是静态

`static` 表示静态。被 `static` 修饰的成员属于类本身，而不是属于某一个具体对象。

普通成员变量和普通成员方法属于对象：

```java
Student student = new Student();
student.study();
```

静态成员变量和静态方法属于类：

```java
MathUtils.add(10, 20);
```

简单来说：

1. 没有 `static` 的成员，通常需要创建对象后使用。
2. 有 `static` 的成员，通常可以直接通过类名使用。

### 10.2 静态变量

静态变量使用 `static` 修饰。

```java
public class Counter {
    public static int count;
}
```

访问静态变量通常使用类名。

```java
Counter.count = 10;
System.out.println(Counter.count);
```

静态变量属于类，所有对象共享同一份静态变量。

```java
public class Counter {
    public static int count;

    public Counter() {
        count++;
    }
}
```

```java
Counter c1 = new Counter();
Counter c2 = new Counter();

System.out.println(Counter.count); // 2
```

`c1` 和 `c2` 是两个不同对象，但 `count` 是类级别数据，只有一份。

### 10.3 静态方法

静态方法使用 `static` 修饰。

```java
public class MathUtils {
    public static int add(int a, int b) {
        return a + b;
    }
}
```

调用静态方法通常使用类名：

```java
int result = MathUtils.add(10, 20);
System.out.println(result); // 30
```

静态方法适合表示“不依赖某个具体对象状态”的工具行为。

静态方法从语法上也可以通过对象调用，但一般不这样写。为了表达该方法属于类本身，通常使用 `类名.方法名(...)` 的形式。

### 10.4 静态方法中的访问限制

静态方法属于类，不属于某个具体对象。因此，静态方法中不能直接访问非静态成员变量和非静态成员方法。

```java
public class Student {
    private String name;

    public static void printName() {
        // System.out.println(name); // 错误：静态方法不能直接访问非静态成员变量
    }
}
```

如果需要访问对象的数据，应先创建对象，再通过对象访问。

```java
public class StudentDemo {
    public static void main(String[] args) {
        Student student = new Student();
        student.study();
    }
}
```

### 10.5 `static` 的一般用法

`static` 常见使用场景包括：

1. 定义工具方法，例如 `MathUtils.add(...)`。
2. 定义常量，例如 `public static final double PI`。
3. 定义所有对象共享的数据，例如对象计数器。
4. 定义程序入口方法 `main`。

示例：

```java
public class MathConfig {
    public static final double PI = 3.1415926;
}
```

```java
System.out.println(MathConfig.PI);
```

`public static final` 常用于定义常量。常量名通常使用全大写字母和下划线。

## 11. 构造方法

### 11.1 什么是构造方法

构造方法用于创建对象时初始化对象数据。

构造方法的特点：

1. 方法名必须与类名完全相同。
2. 没有返回值类型，连 `void` 也不能写。
3. 使用 `new` 创建对象时自动调用。

### 11.2 怎么写构造方法

示例：

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

创建对象：

```java
Student student = new Student("LiMing", 18);
```

`new Student("LiMing", 18)` 会调用 `Student(String name, int age)` 构造方法。

### 11.3 无参构造方法

如果类中没有手动定义任何构造方法，Java 会自动提供一个无参构造方法。

```java
public class Student {
}
```

可以这样创建对象：

```java
Student student = new Student();
```

如果手动定义了构造方法，Java 不再自动提供无参构造方法。

```java
public class Student {
    public Student(String name) {
    }
}
```

此时以下写法错误：

```java
// Student student = new Student(); // 错误
```

### 11.4 `this`

`this` 表示当前对象。

```java
public class Student {
    private String name;

    public Student(String name) {
        this.name = name;
    }
}
```

`this.name` 表示当前对象的成员变量，右侧的 `name` 表示构造方法参数。

### 11.5 构造方法重载

一个类中可以定义多个构造方法，只要参数列表不同即可。这称为构造方法重载。

```java
public class Student {
    private String name;
    private int age;

    public Student() {
        this.name = "Unknown";
        this.age = 0;
    }

    public Student(String name) {
        this.name = name;
        this.age = 0;
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

创建对象时，Java 会根据传入参数选择对应的构造方法。

```java
Student s1 = new Student();
Student s2 = new Student("LiMing");
Student s3 = new Student("ZhangSan", 18);
```

构造方法之间也可以使用 `this(...)` 调用同一个类中的其他构造方法。`this(...)` 必须写在构造方法的第一条语句位置。

```java
public class Student {
    private String name;
    private int age;

    public Student() {
        this("Unknown", 0);
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

## 12. 方法的重载

### 12.1 方法重载的含义

在同一个类中，多个方法可以具有相同方法名，但参数列表必须不同。这称为方法重载（Overload）。

```java
public class Printer {
    public void print(String text) {
        System.out.println(text);
    }

    public void print(int value) {
        System.out.println(value);
    }
}
```

### 12.2 参数列表不同

参数列表不同包括：

1. 参数个数不同。
2. 参数类型不同。
3. 参数顺序不同。

示例：

```java
public void show(String name) {
}

public void show(String name, int age) {
}

public void show(int age, String name) {
}
```

仅返回值类型不同不能构成重载。

```java
// public int getValue() { return 1; }
// public double getValue() { return 1.0; } // 错误
```

## 13. 封装

### 13.1 什么是封装

封装是指将数据和操作数据的方法放在类中，并通过访问权限限制外部代码直接操作数据。

封装的目标是：

1. 隐藏对象内部实现细节。
2. 保护数据不被随意修改。
3. 对外提供清晰、稳定的使用方式。

### 13.2 封装的思想如何使用

典型封装写法：

1. 成员变量使用 `private` 修饰。
2. 提供 `public` 的 `getter` 方法读取数据。
3. 提供 `public` 的 `setter` 方法修改数据。
4. 在 `setter` 方法中进行必要校验。

示例：

```java
public class Student {
    private String name;
    private double score;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        if (score >= 0 && score <= 100) {
            this.score = score;
        }
    }
}
```

外部代码不能直接访问 `score`，只能通过 `setScore` 修改。这样可以防止 `-100`、`200` 等不合理成绩进入对象。

## 14. 继承

### 14.1 什么是继承

继承用于表示类与类之间的父子关系。子类可以继承父类已有的成员，并在父类基础上扩展新的内容。

继承适合表达“子类是父类的一种”。

```text
Cat 是 Animal 的一种
Dog 是 Animal 的一种
Teacher 是 Person 的一种
```

### 14.2 如何继承

Java 使用 `extends` 表示继承。

```java
public class Animal {
    public void eat() {
        System.out.println("吃饭");
    }
}
```

```java
public class Cat extends Animal {
    public void catchMouse() {
        System.out.println("抓老鼠");
    }
}
```

使用：

```java
Cat cat = new Cat();
cat.eat();
cat.catchMouse();
```

`Cat` 继承了 `Animal` 的 `eat` 方法，同时又拥有自己的 `catchMouse` 方法。

### 14.3 `super`

`super` 表示父类部分，常用于在子类中访问父类成员或调用父类构造方法。

调用父类构造方法：

```java
public class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }
}
```

```java
public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
}
```

`super(name)` 表示调用父类 `Animal` 的构造方法。它必须写在子类构造方法的第一条语句位置。

构造方法不能被继承。子类创建对象时，会先完成父类部分的初始化，因此子类构造方法中需要直接或间接调用父类构造方法。

如果子类构造方法中没有显式写出 `super(...)`，Java 会默认尝试调用父类的无参构造方法。

```java
public class Animal {
    public Animal() {
    }
}

public class Cat extends Animal {
    public Cat() {
        // 默认相当于调用 super();
    }
}
```

如果父类没有无参构造方法，子类就必须显式使用 `super(...)` 调用父类已有的构造方法。

调用父类方法：

```java
public class Animal {
    public void eat() {
        System.out.println("动物吃饭");
    }
}
```

```java
public class Cat extends Animal {
    public void eatFish() {
        super.eat();
        System.out.println("猫吃鱼");
    }
}
```

### 14.4 继承中的 `protected`

`protected` 常用于父类成员。被 `protected` 修饰的成员可以在同一个包中访问，也可以在子类中访问。

```java
public class Animal {
    protected String name;

    protected void eat() {
        System.out.println(name + " 正在吃饭");
    }
}
```

```java
public class Cat extends Animal {
    public void showInfo() {
        name = "Mimi";
        eat();
    }
}
```

在继承关系中，`protected` 比 `private` 更适合给子类使用。`private` 成员只能在当前类内部直接访问，子类不能直接访问父类的 `private` 成员。

```java
public class Animal {
    private int age;
}
```

```java
public class Cat extends Animal {
    public void showAge() {
        // System.out.println(age); // 错误：age 是父类 private 成员
    }
}
```

如果父类希望子类读取或修改 `private` 成员，通常通过 `public` 或 `protected` 方法间接提供访问。

### 14.5 Java 单继承

Java 类只支持单继承，一个类只能直接继承一个父类。

```java
public class Cat extends Animal {
}
```

以下写法不允许：

```java
// public class Cat extends Animal, Pet {
// }
```

### 14.6 `final` 类

`final` 修饰的类不能被继承。

```java
public final class StringUtils {
}
```

如果一个类不希望被其他类扩展，可以使用 `final`。

```java
// public class MyStringUtils extends StringUtils {
// } // 错误：StringUtils 是 final 类，不能被继承
```

## 15. 多态

### 15.1 什么是多态

多态是指同一个父类引用或接口引用，可以指向不同子类对象，并在运行时表现出不同执行效果。

```java
Animal animal = new Cat();
```

左侧类型是 `Animal`，右侧实际对象是 `Cat`。

### 15.2 多态示例

```java
public class Animal {
    public void makeSound() {
        System.out.println("动物发出声音");
    }
}
```

```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("猫叫");
    }
}
```

```java
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("狗叫");
    }
}
```

使用：

```java
Animal a1 = new Cat();
Animal a2 = new Dog();

a1.makeSound(); // 猫叫
a2.makeSound(); // 狗叫
```

变量类型都是 `Animal`，但实际对象不同，执行结果不同。

多态可以概括为：编译看左边，运行看右边。

### 15.3 多态引用的访问范围

多态中，变量声明类型决定编译阶段能调用哪些成员。

```java
Animal animal = new Cat();
animal.makeSound();
```

如果 `Cat` 中有 `catchMouse` 方法，而 `Animal` 中没有该方法，则不能直接通过 `animal` 调用。

```java
Animal animal = new Cat();
// animal.catchMouse(); // 错误：Animal 类型中没有 catchMouse 方法
```

如果确实需要调用子类特有方法，需要进行类型转换。

```java
Animal animal = new Cat();

if (animal instanceof Cat) {
    Cat cat = (Cat) animal;
    cat.catchMouse();
}
```

这说明多态适合调用父类或接口中统一定义的方法；子类特有方法不应被随意依赖。

多态主要讨论方法调用的运行效果。成员变量不具备方法重写那样的多态效果，因此在实际编写代码时，应尽量通过方法暴露对象行为，而不是依赖同名成员变量。

## 16. 方法的重写

### 16.1 方法重写的含义

方法重写是指子类重新定义从父类继承来的方法。

```java
public class Animal {
    public void makeSound() {
        System.out.println("动物发出声音");
    }
}
```

```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("猫叫");
    }
}
```

### 16.2 `@Override`

`@Override` 用于标记该方法是重写父类方法。

```java
@Override
public void makeSound() {
    System.out.println("猫叫");
}
```

如果方法名、参数列表等写错，编译器会提示错误。建议重写方法时都写 `@Override`。

### 16.3 方法重写规则

方法重写应满足以下基本规则：

1. 子类方法名必须与父类方法名相同。
2. 子类方法参数列表必须与父类方法参数列表相同。
3. 子类方法返回值类型应与父类方法返回值类型相同，或是父类返回值类型的子类型。
4. 子类方法访问权限不能比父类方法更严格。
5. `final` 方法不能被重写。
6. `private` 方法不能被子类直接重写。
7. `static` 方法不属于对象多态中的方法重写。

示例：

```java
public class Animal {
    public void makeSound() {
        System.out.println("动物发出声音");
    }
}
```

```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("猫叫");
    }
}
```

### 16.4 重写与重载的区别

| 对比项   | 方法重载         | 方法重写             |
| -------- | ---------------- | -------------------- |
| 发生位置 | 同一个类中       | 父类与子类之间       |
| 方法名   | 相同             | 相同                 |
| 参数列表 | 必须不同         | 必须相同             |
| 作用     | 提供多种调用方式 | 改变继承来的方法行为 |

## 17. 抽象类

### 17.1 什么是抽象类

抽象类是使用 `abstract` 修饰的类。抽象类通常用于描述不完整的父类模型。

例如，“动物”这个概念可以作为父类，但现实中通常不会创建一个泛泛的“动物对象”，而是创建猫、狗、鸟等具体对象。

抽象类可以没有抽象方法。但是，如果一个类中定义了抽象方法，那么该类必须声明为抽象类。

### 17.2 抽象类怎么定义

```java
public abstract class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

抽象类不能直接创建对象。

```java
// Animal animal = new Animal("animal"); // 错误
```

### 17.3 抽象类中的抽象方法

抽象方法使用 `abstract` 修饰，只有方法声明，没有方法体。

```java
public abstract void makeSound();
```

抽象方法必须写在抽象类或接口中。

示例：

```java
public abstract class Animal {
    public abstract void makeSound();
}
```

如果一个类继承抽象类，就必须实现抽象类中的抽象方法，除非该子类本身也是抽象类。

```java
public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("猫叫");
    }
}
```

抽象方法不能使用 `private` 修饰，因为子类需要能够重写它。

抽象方法也不能使用 `final` 修饰，因为 `final` 方法不能被重写；抽象方法也不能使用 `static` 修饰，因为静态方法属于类本身，不参与对象多态。

抽象类本身也不能同时使用 `final` 修饰。`abstract` 表示该类需要被子类继承并补全，`final` 表示该类不能被继承，二者含义冲突。

### 17.4 抽象类怎么使用

抽象类通常通过子类使用。

```java
Animal animal = new Cat();
animal.makeSound();
```

抽象类可以包含：

1. 成员变量。
2. 构造方法。
3. 普通方法。
4. 抽象方法。

抽象类的构造方法不是用于直接创建抽象类对象，而是供子类构造对象时调用。

## 18. 接口

### 18.1 什么是接口

接口用于描述一组能力或规则。接口关注“能做什么”，而不是“是什么”。

例如：

```text
会飞
会游泳
可支付
可打印
```

这些能力可以被不同类实现。

### 18.2 接口怎么声明

Java 使用 `interface` 声明接口。

```java
public interface Flyable {
}
```

接口名通常也使用大驼峰命名法。

### 18.3 接口中怎么写方法

接口中的普通抽象方法可以只写方法声明。

```java
public interface Flyable {
    void fly();
}
```

接口中的方法默认是 `public abstract`。因此，上面的写法等价于：

```java
public interface Flyable {
    public abstract void fly();
}
```

接口中也可以定义常量。

```java
public interface Config {
    int MAX_COUNT = 100;
}
```

接口中的常量默认是 `public static final`。

实现接口方法时，方法必须使用 `public`。因为接口中的抽象方法默认是 `public`，实现类不能降低访问权限。

```java
public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("鸟在飞");
    }
}
```

接口还可以包含 `default` 方法和 `static` 方法，但本任务重点掌握接口中的抽象方法和常量。

### 18.4 接口怎么使用

类使用 `implements` 实现接口。

```java
public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("鸟在飞");
    }
}
```

实现接口的类必须实现接口中的抽象方法，除非该类本身是抽象类。

调用：

```java
Bird bird = new Bird();
bird.fly();
```

接口也是引用类型，可以用接口引用指向实现类对象。

```java
Flyable flyable = new Bird();
flyable.fly();
```

接口不能直接创建对象。

```java
// Flyable flyable = new Flyable(); // 错误
```

一个类可以实现多个接口。

```java
public class Duck implements Flyable, Swimmable {
}
```

接口也可以继承接口，并且一个接口可以继承多个接口。

```java
public interface Moveable {
    void move();
}

public interface Trackable {
    void track();
}

public interface Flyable extends Moveable, Trackable {
    void fly();
}
```

## 19. 抽象类和接口的比较

抽象类和接口都可以包含抽象方法，都不能直接创建对象，都可以作为多态引用类型。

| 对比项   | 抽象类                     | 接口                                                            |
| -------- | -------------------------- | --------------------------------------------------------------- |
| 关键字   | `abstract class`           | `interface`                                                     |
| 使用方式 | `extends` 继承             | `implements` 实现                                               |
| 关系含义 | 是某类事物                 | 具备某种能力                                                    |
| 数量限制 | 一个类只能直接继承一个父类 | 一个类可以实现多个接口                                          |
| 构造方法 | 可以有构造方法             | 不能定义构造方法                                                |
| 成员变量 | 可以定义普通成员变量       | 主要定义 `public static final` 常量                             |
| 普通方法 | 可以定义普通方法           | 可以定义 `default` 方法和 `static` 方法，本任务重点掌握抽象方法 |

使用建议：

1. 如果要描述“是什么”，优先考虑抽象类或普通父类。
2. 如果要描述“能做什么”，优先考虑接口。

示例：

```text
Bird 是 Animal 的一种
Bird 具备 Flyable 能力
```

对应代码：

```java
public class Bird extends Animal implements Flyable {
}
```

## 20. 内部类

### 20.1 什么是内部类

内部类是定义在另一个类内部的类。

```java
public class Outer {
    public class Inner {
    }
}
```

其中，`Inner` 定义在 `Outer` 内部，因此 `Inner` 是内部类。

### 20.2 内部类怎么定义

成员内部类示例：

```java
public class Computer {
    private String brand = "Lenovo";

    public class Cpu {
        public void showInfo() {
            System.out.println("CPU belongs to " + brand);
        }
    }
}
```

使用成员内部类：

```java
Computer computer = new Computer();
Computer.Cpu cpu = computer.new Cpu();
cpu.showInfo();
```

静态内部类示例：

```java
public class Computer {
    public static class UsbPort {
        public void connect() {
            System.out.println("USB connected");
        }
    }
}
```

使用静态内部类：

```java
Computer.UsbPort port = new Computer.UsbPort();
port.connect();
```

### 20.3 内部类的访问修饰符

内部类可以使用 `public`、`protected`、默认、`private` 修饰。

```java
public class Outer {
    public class PublicInner {
    }

    protected class ProtectedInner {
    }

    class DefaultInner {
    }

    private class PrivateInner {
    }
}
```

`private` 内部类只能在外部类内部使用。

```java
public class Outer {
    private class Inner {
        void show() {
            System.out.println("inner");
        }
    }

    public void createInner() {
        Inner inner = new Inner();
        inner.show();
    }
}
```

`protected` 内部类可以在同一个包中访问，也可以在子类中访问。它常用于需要让子类了解某个内部结构，但不希望完全公开给所有外部代码的场景。

顶层类不能使用 `private` 或 `protected` 修饰，内部类可以使用，这是类修饰符中容易混淆的地方。

### 20.4 内部类的使用场景

内部类适合用于表示强依附关系。

例如：

1. `Computer` 和 `Cpu`。
2. `Car` 和 `Engine`。
3. `Order` 和 `OrderItem`。

如果一个类只服务于另一个类，并且不需要被外部广泛使用，可以考虑定义为内部类。

一般来说，只需要先理解：内部类是定义在类内部的类，它可以让关系紧密的类组织在一起。

Java 中还存在局部内部类和匿名内部类等形式。本任务重点掌握成员内部类和静态内部类，其他形式可以在后续任务中再展开。

## 21. 命令行参数

### 21.1 `main` 方法

Java 程序入口方法通常写作：

```java
public static void main(String[] args) {
}
```

其中：

1. `public` 表示该方法可以被外部调用。
2. `static` 表示该方法属于类，不需要先创建对象即可调用。
3. `void` 表示该方法没有返回值。
4. `main` 是固定方法名。
5. `String[] args` 是方法参数。

### 21.2 `String[] args` 的含义

`String[] args` 表示一个字符串数组参数，用于接收程序启动时传入的命令行参数。

```java
public class ArgsDemo {
    public static void main(String[] args) {
        System.out.println(args.length);
    }
}
```

如果从命令行运行程序时传入参数：

```bat
java ArgsDemo hello java
```

那么：

1. `args[0]` 是 `"hello"`。
2. `args[1]` 是 `"java"`。
3. `args.length` 是 `2`。

在基础程序中通常不会频繁使用 `args`，但应知道它是 `main` 方法的固定参数形式。

## 22. 代码示例

在 IDEA 中创建以下文件，并保证它们位于同一个包或同一个源码目录下。

### 22.1 `Animal.java`

```java
public abstract class Animal {
    private String name;

    public Animal(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void eat() {
        System.out.println(name + " 正在吃饭");
    }

    public abstract void makeSound();
}
```

### 22.2 `Flyable.java`

```java
public interface Flyable {
    void fly();
}
```

### 22.3 `Cat.java`

```java
public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println(getName() + " 喵喵叫");
    }

    public void catchMouse() {
        System.out.println(getName() + " 正在抓老鼠");
    }
}
```

### 22.4 `Bird.java`

```java
public class Bird extends Animal implements Flyable {
    public Bird(String name) {
        super(name);
    }

    @Override
    public void makeSound() {
        System.out.println(getName() + " 叽叽叫");
    }

    @Override
    public void fly() {
        System.out.println(getName() + " 正在飞行");
    }
}
```

### 22.5 `OopDemo.java`

```java
public class OopDemo {
    public static void main(String[] args) {
        Cat cat = new Cat("Mimi");
        cat.eat();
        cat.makeSound();
        cat.catchMouse();

        Bird bird = new Bird("Bibi");
        bird.eat();
        bird.makeSound();
        bird.fly();

        Animal animal1 = new Cat("Kitty");
        Animal animal2 = new Bird("Dudu");

        animal1.makeSound();
        animal2.makeSound();

        Flyable flyable = new Bird("Feifei");
        flyable.fly();
    }
}
```

运行后应重点观察以下内容：

1. `Animal` 为什么定义为抽象类。
2. `Cat` 和 `Bird` 如何继承 `Animal`。
3. `Bird` 如何实现 `Flyable` 接口。
4. `makeSound` 如何在不同子类中产生不同执行结果。
5. `Animal animal = new Cat(...)` 如何体现多态。
6. `Flyable flyable = new Bird(...)` 如何体现接口引用。

## 23. 练习

### 23.1 类、方法与封装练习

创建 `Student.java`，完成以下要求：

1. 定义 `Student` 类。
2. 定义 `private String name`。
3. 定义 `private int age`。
4. 定义 `private double score`。
5. 定义有参构造方法，用于初始化三个成员变量。
6. 定义 `getName`、`getAge`、`getScore` 方法。
7. 定义 `setScore` 方法，要求成绩只能在 `0` 到 `100` 之间。
8. 定义 `getLevel` 方法，根据成绩返回 `优秀`、`良好`、`及格` 或 `不及格`。

### 23.2 继承、重写与多态练习

创建 `Person.java` 和 `Teacher.java`，完成以下要求：

1. `Person` 类包含 `name` 和 `age`。
2. `Person` 类包含 `introduce` 方法。
3. `Teacher` 类继承 `Person`。
4. `Teacher` 类新增 `subject` 成员变量。
5. `Teacher` 类重写 `introduce` 方法。
6. 使用 `Person person = new Teacher(...);` 调用 `introduce` 方法。

### 23.3 抽象类与接口练习

创建 `Playable.java`、`Player.java` 和 `BasketballPlayer.java`，完成以下要求：

1. `Playable` 是接口。
2. `Playable` 中定义 `play` 方法。
3. `Player` 是抽象类。
4. `Player` 中定义 `name` 成员变量和抽象方法 `train`。
5. `BasketballPlayer` 继承 `Player`。
6. `BasketballPlayer` 实现 `Playable` 接口。
7. `BasketballPlayer` 实现 `train` 方法和 `play` 方法。
8. 使用 `Playable player = new BasketballPlayer(...);` 调用 `play` 方法。

### 23.4 内部类练习

创建 `Computer.java`，完成以下要求：

1. 定义 `Computer` 类。
2. 定义成员变量 `brand`。
3. 在 `Computer` 内部定义成员内部类 `Cpu`。
4. 在 `Cpu` 中定义 `showInfo` 方法。
5. 创建 `Computer` 对象。
6. 通过 `Computer` 对象创建 `Cpu` 对象。
7. 调用 `showInfo` 方法。

### 23.5 综合练习

创建 `OopPractice.java`，完成以下要求：

1. 创建 `Student` 对象并输出学生信息。
2. 创建 `Teacher` 对象并观察方法重写结果。
3. 创建 `BasketballPlayer` 对象并调用 `train` 和 `play` 方法。
4. 使用父类引用指向子类对象，观察多态效果。
5. 使用接口引用指向实现类对象，观察接口多态效果。
6. 创建并使用 `Computer.Cpu` 内部类对象。

## 24. 验收标准

满足以下条件视为完成 Task 3：

1. 能说明类用于描述对象本身，方法用于描述对象可以执行的动作。
2. 能说明包的作用，并能写出 `package` 和 `import`。
3. 能正确定义类，并说明类名和文件名之间的关系。
4. 能说明常见类修饰符。
5. 能定义成员变量和成员方法。
6. 能说明成员访问修饰符的访问范围。
7. 能说明常见方法修饰符。
8. 能说明 `static` 的含义、效果和常见用途。
9. 能定义构造方法，并说明构造方法与普通方法的区别。
10. 能写出方法重载。
11. 能使用 `private`、`getter`、`setter` 完成封装。
12. 能说明方法签名、形参和实参的基本含义。
13. 能写出构造方法重载，并能说明 `this(...)` 的基本作用。
14. 能使用 `extends` 定义继承关系，并能说明 `super` 的基本作用。
15. 能说明多态的基本含义和多态引用的访问范围。
16. 能使用 `@Override` 完成方法重写，并能说明方法重写的基本规则。
17. 能定义抽象类和抽象方法。
18. 能定义接口并使用 `implements` 实现接口。
19. 能说明抽象类和接口的主要区别。
20. 能定义并使用基础内部类。
21. 能说明 `String[] args` 的基本含义。
22. 能独立完成 `OopPractice.java` 并运行得到正确结果。

## 25. 提交规范

1. 学习/实验总结报告统一命名为 `Task_3_report.md`，放置在 `Task_3/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_3/src/` 目录下（或在 `Task_3/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_3/` 目录中的本阶段内容。
