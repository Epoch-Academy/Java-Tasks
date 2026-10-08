# Task 4：Java 泛型、集合、包装类与常用工具类

## 1. 任务说明

本任务引入 Java 中非常常用的一组基础能力，重点包括包装类、泛型、常见集合类，以及不涉及异常和 IO 的常用工具类。

在前面的任务中，已经学习了变量、流程控制、数组和面向对象基础。数组可以保存多个同类型数据，但数组长度固定，功能也比较有限。在实际开发中，更常使用集合来保存、查找、遍历和管理一组对象。

集合通常会配合泛型使用。泛型可以让集合明确保存什么类型的数据，从而减少类型转换错误，并提高代码可读性。

本任务不讲解异常处理，也不讲解文件、网络、输入输出流等 IO 内容。代码示例应尽量使用固定数据完成演示，不依赖键盘输入或文件读写。

## 2. 知识范围

本任务包含以下内容：

1. 包装类。
2. 自动装箱与自动拆箱。
3. 包装类常用方法。
4. 泛型的基本含义。
5. 泛型类、泛型方法和泛型接口。
6. 泛型通配符。
7. 集合框架基础。
8. `List`、`ArrayList`、`LinkedList`。
9. `Set`、`HashSet`、`LinkedHashSet`、`TreeSet`。
10. `Map`、`HashMap`、`LinkedHashMap`、`TreeMap`。
11. 集合遍历方式。
12. 集合排序与比较器。
13. `Collections` 工具类。
14. `Arrays` 工具类。
15. `Objects` 工具类。
16. `Math` 工具类。
17. `Random` 工具类。
18. `StringBuilder` 与 `StringJoiner`。
19. `BigDecimal`。

## 3. 包装类

### 3.1 什么是包装类

Java 中有 8 种基本类型：

| 基本类型 | 含义     |
| -------- | -------- |
| `byte`   | 字节整数 |
| `short`  | 短整数   |
| `int`    | 整数     |
| `long`   | 长整数   |
| `float`  | 单精度浮点数 |
| `double` | 双精度浮点数 |
| `char`   | 字符     |
| `boolean` | 布尔值  |

基本类型不是对象，不能直接调用方法。为了让基本类型也能以对象形式使用，Java 为每种基本类型提供了对应的包装类。

| 基本类型 | 包装类      |
| -------- | ----------- |
| `byte`   | `Byte`      |
| `short`  | `Short`     |
| `int`    | `Integer`   |
| `long`   | `Long`      |
| `float`  | `Float`     |
| `double` | `Double`    |
| `char`   | `Character` |
| `boolean` | `Boolean`  |

示例：

```java
Integer number = Integer.valueOf(100);
Double price = Double.valueOf(19.9);
Character level = Character.valueOf('A');
Boolean passed = Boolean.valueOf(true);
```

其中：

1. `Integer` 是 `int` 的包装类。
2. `Double` 是 `double` 的包装类。
3. `Character` 是 `char` 的包装类。
4. `Boolean` 是 `boolean` 的包装类。

### 3.2 为什么需要包装类

包装类常见用途包括：

1. 在集合中保存基本类型数据。
2. 调用包装类提供的方法。
3. 完成字符串和基本类型之间的转换。
4. 使用对象形式表示一个基本类型值。

集合只能保存对象，不能直接保存基本类型。

```java
ArrayList<Integer> scores = new ArrayList<>();
scores.add(95);
scores.add(88);
```

这里的 `ArrayList<Integer>` 表示集合中保存的是 `Integer` 对象。虽然代码中写入的是 `95` 和 `88`，但 Java 会自动将 `int` 转换为 `Integer`。

### 3.3 自动装箱

自动装箱是指 Java 自动把基本类型转换为对应包装类对象。

```java
Integer number = 100;
Double price = 19.9;
Boolean passed = true;
```

上述代码等价于：

```java
Integer number = Integer.valueOf(100);
Double price = Double.valueOf(19.9);
Boolean passed = Boolean.valueOf(true);
```

自动装箱让代码更简洁，但仍应理解底层含义：基本类型值被包装成了对象。

### 3.4 自动拆箱

自动拆箱是指 Java 自动把包装类对象转换为对应基本类型值。

```java
Integer number = 100;
int result = number + 20;
System.out.println(result);
```

`number + 20` 执行时，`number` 会自动拆箱为 `int`，然后再参与计算。

示例：

```java
public class BoxingDemo {
    public static void main(String[] args) {
        Integer a = 10;
        Integer b = 20;

        int sum = a + b;

        System.out.println(sum);
    }
}
```

其中：

1. `Integer a = 10;` 发生自动装箱。
2. `a + b` 发生自动拆箱。
3. `sum` 保存基本类型计算结果。

### 3.5 包装类常用方法

包装类提供了一些常用方法和常量。

```java
public class WrapperMethodDemo {
    public static void main(String[] args) {
        int number = Integer.parseInt("123");
        double price = Double.parseDouble("19.9");
        boolean flag = Boolean.parseBoolean("true");

        System.out.println(number + 1);
        System.out.println(price + 0.1);
        System.out.println(flag);

        System.out.println(Integer.MAX_VALUE);
        System.out.println(Integer.MIN_VALUE);
    }
}
```

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `Integer.parseInt(String value)` | 将字符串转换为 `int` |
| `Double.parseDouble(String value)` | 将字符串转换为 `double` |
| `Boolean.parseBoolean(String value)` | 将字符串转换为 `boolean` |
| `Integer.valueOf(int value)` | 将 `int` 转换为 `Integer` |
| `Integer.toString(int value)` | 将 `int` 转换为 `String` |

注意：本任务只使用格式正确的字符串进行转换，不讨论格式不正确的情况。

### 3.6 包装类比较

包装类是对象。比较包装类对象时，应区分比较引用和比较内容。

```java
Integer a = 1000;
Integer b = 1000;

System.out.println(a == b);
System.out.println(a.equals(b));
```

其中：

1. `==` 比较的是两个引用是否指向同一个对象。
2. `equals` 比较的是两个对象表示的值是否相同。

对于包装类对象，通常使用 `equals` 比较内容。

```java
Integer a = 1000;
Integer b = 1000;

if (a.equals(b)) {
    System.out.println("值相同");
}
```

## 4. 泛型

### 4.1 什么是泛型

泛型用于把类型作为参数传递给类、接口或方法。

没有泛型时，一个容器可能保存各种类型的数据，取出数据时需要强制类型转换。

```java
ArrayList list = new ArrayList();
list.add("Java");

String value = (String) list.get(0);
System.out.println(value);
```

使用泛型后，可以明确集合中保存的数据类型。

```java
ArrayList<String> list = new ArrayList<>();
list.add("Java");

String value = list.get(0);
System.out.println(value);
```

其中：

1. `ArrayList<String>` 表示这个集合只能保存 `String` 类型数据。
2. `list.get(0)` 取出的结果直接是 `String`。
3. 不需要手动进行强制类型转换。

泛型可以提升代码安全性和可读性。

### 4.2 泛型的基本写法

泛型通常写在类名或接口名后面的尖括号中。

```java
ArrayList<String> names = new ArrayList<>();
ArrayList<Integer> scores = new ArrayList<>();
HashMap<String, Integer> scoreMap = new HashMap<>();
```

其中：

1. `ArrayList<String>` 表示元素类型是 `String`。
2. `ArrayList<Integer>` 表示元素类型是 `Integer`。
3. `HashMap<String, Integer>` 表示键是 `String`，值是 `Integer`。

右侧的 `<>` 称为菱形语法。Java 可以根据左侧泛型类型推断右侧类型。

```java
ArrayList<String> names = new ArrayList<>();
```

上述写法等价于：

```java
ArrayList<String> names = new ArrayList<String>();
```

### 4.3 泛型只能使用引用类型

泛型中不能直接写基本类型。

错误写法：

```java
ArrayList<int> numbers = new ArrayList<>();
```

正确写法：

```java
ArrayList<Integer> numbers = new ArrayList<>();
```

原因是泛型只能接收引用类型。基本类型需要使用对应包装类。

常见写法：

```java
ArrayList<Integer> integers = new ArrayList<>();
ArrayList<Double> doubles = new ArrayList<>();
ArrayList<Character> characters = new ArrayList<>();
ArrayList<Boolean> booleans = new ArrayList<>();
```

### 4.4 泛型类

泛型类是在定义类时使用类型参数。

```java
public class Box<T> {
    private T value;

    public Box(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }

    public void setValue(T value) {
        this.value = value;
    }
}
```

其中：

1. `T` 是类型参数。
2. `private T value` 表示成员变量类型由创建对象时决定。
3. `public T getValue()` 表示返回值类型由创建对象时决定。

使用泛型类：

```java
public class BoxDemo {
    public static void main(String[] args) {
        Box<String> nameBox = new Box<>("Java");
        Box<Integer> scoreBox = new Box<>(95);

        String name = nameBox.getValue();
        Integer score = scoreBox.getValue();

        System.out.println(name);
        System.out.println(score);
    }
}
```

`Box<String>` 中的 `T` 会被当作 `String` 使用；`Box<Integer>` 中的 `T` 会被当作 `Integer` 使用。

### 4.5 泛型方法

泛型方法是在方法返回值类型前声明类型参数。

```java
public class GenericMethodDemo {
    public static <T> void printValue(T value) {
        System.out.println(value);
    }

    public static <T> T getFirst(T[] array) {
        return array[0];
    }

    public static void main(String[] args) {
        printValue("Java");
        printValue(100);
        printValue(19.9);

        String[] names = {"Alice", "Bob"};
        Integer[] scores = {95, 88};

        System.out.println(getFirst(names));
        System.out.println(getFirst(scores));
    }
}
```

其中：

1. `<T>` 表示声明一个类型参数。
2. `void printValue(T value)` 表示参数类型由调用时决定。
3. `T getFirst(T[] array)` 表示返回值类型与数组元素类型一致。

### 4.6 泛型接口

泛型接口是在接口名后声明类型参数。

```java
public interface Repository<T> {
    void save(T value);

    T findFirst();
}
```

实现泛型接口时，可以指定具体类型。

```java
public class StringRepository implements Repository<String> {
    private String value;

    @Override
    public void save(String value) {
        this.value = value;
    }

    @Override
    public String findFirst() {
        return value;
    }
}
```

使用示例：

```java
public class RepositoryDemo {
    public static void main(String[] args) {
        Repository<String> repository = new StringRepository();
        repository.save("Java");

        String value = repository.findFirst();
        System.out.println(value);
    }
}
```

### 4.7 常见类型参数命名

泛型类型参数可以使用任意合法标识符，但通常使用单个大写字母。

| 类型参数 | 常见含义 |
| -------- | -------- |
| `T` | Type，普通类型 |
| `E` | Element，集合元素 |
| `K` | Key，键 |
| `V` | Value，值 |
| `N` | Number，数字 |

示例：

```java
public class Pair<K, V> {
    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }
}
```

### 4.8 泛型通配符

通配符使用 `?` 表示未知类型。

```java
public static void printList(ArrayList<?> list) {
    for (Object value : list) {
        System.out.println(value);
    }
}
```

`ArrayList<?>` 表示可以接收任意元素类型的 `ArrayList`。

示例：

```java
import java.util.ArrayList;

public class WildcardDemo {
    public static void printList(ArrayList<?> list) {
        for (Object value : list) {
            System.out.println(value);
        }
    }

    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");

        ArrayList<Integer> scores = new ArrayList<>();
        scores.add(95);
        scores.add(88);

        printList(names);
        printList(scores);
    }
}
```

### 4.9 泛型上限

泛型上限用于限制类型必须是某个类或其子类。

```java
public static double sum(ArrayList<? extends Number> numbers) {
    double total = 0;

    for (Number number : numbers) {
        total += number.doubleValue();
    }

    return total;
}
```

`? extends Number` 表示接收 `Number` 或 `Number` 的子类，例如 `Integer`、`Double`。

完整示例：

```java
import java.util.ArrayList;

public class UpperBoundDemo {
    public static double sum(ArrayList<? extends Number> numbers) {
        double total = 0;

        for (Number number : numbers) {
            total += number.doubleValue();
        }

        return total;
    }

    public static void main(String[] args) {
        ArrayList<Integer> integers = new ArrayList<>();
        integers.add(10);
        integers.add(20);

        ArrayList<Double> doubles = new ArrayList<>();
        doubles.add(1.5);
        doubles.add(2.5);

        System.out.println(sum(integers));
        System.out.println(sum(doubles));
    }
}
```

本任务只要求理解泛型通配符和泛型上限的基本含义，不要求深入掌握复杂泛型设计。

## 5. 集合框架基础

### 5.1 什么是集合

集合是 Java 提供的一组用于保存多个对象的容器。

数组也能保存多个数据，但数组有明显限制：

1. 数组长度固定。
2. 数组提供的方法较少。
3. 删除、插入、查找等操作需要自己编写较多代码。

集合可以更灵活地保存对象，并提供大量常用方法。

```java
ArrayList<String> names = new ArrayList<>();
names.add("Alice");
names.add("Bob");
names.add("Cindy");

System.out.println(names.size());
System.out.println(names.get(0));
```

### 5.2 集合和数组的区别

| 对比项 | 数组 | 集合 |
| ------ | ---- | ---- |
| 长度 | 创建后固定 | 可动态变化 |
| 保存内容 | 基本类型或引用类型 | 引用类型 |
| 常用操作 | 主要依赖索引 | 提供丰富方法 |
| 泛型支持 | 不使用泛型 | 常配合泛型使用 |

数组适合保存长度明确、结构简单的数据。集合适合保存数量经常变化、需要增删查改的数据。

### 5.3 集合体系

Java 集合框架中常见接口包括：

1. `Collection`：单列集合的顶层接口。
2. `List`：有序、可重复、有索引。
3. `Set`：不允许重复，通常没有索引。
4. `Map`：双列集合，用键值对保存数据。

常见集合类：

| 接口 | 常见实现类 | 主要特点 |
| ---- | ---------- | -------- |
| `List` | `ArrayList` | 有序、可重复、查询方便 |
| `List` | `LinkedList` | 有序、可重复，适合头尾操作 |
| `Set` | `HashSet` | 不重复，不保证顺序 |
| `Set` | `LinkedHashSet` | 不重复，保留添加顺序 |
| `Set` | `TreeSet` | 不重复，可排序 |
| `Map` | `HashMap` | 键不重复，不保证顺序 |
| `Map` | `LinkedHashMap` | 键不重复，保留添加顺序 |
| `Map` | `TreeMap` | 键不重复，按键排序 |

### 5.4 `Collection` 常用方法

`Collection` 是 `List` 和 `Set` 的父接口。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `add(E e)` | 添加元素 |
| `remove(Object o)` | 删除指定元素 |
| `contains(Object o)` | 判断是否包含元素 |
| `size()` | 获取元素个数 |
| `isEmpty()` | 判断集合是否为空 |
| `clear()` | 清空集合 |

示例：

```java
import java.util.ArrayList;

public class CollectionMethodDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        System.out.println(names.contains("Bob"));
        System.out.println(names.size());

        names.remove("Bob");

        System.out.println(names);
        System.out.println(names.isEmpty());
    }
}
```

## 6. `List`

### 6.1 `List` 的特点

`List` 是有序、可重复、有索引的集合。

有序表示元素会按照添加顺序保存。

可重复表示相同元素可以出现多次。

有索引表示可以通过位置访问元素。

```java
import java.util.ArrayList;

public class ListDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Alice");

        System.out.println(names.get(0));
        System.out.println(names.get(1));
        System.out.println(names.get(2));
    }
}
```

输出中可以看到 `"Alice"` 出现了两次，并且顺序与添加顺序一致。

### 6.2 `List` 常用方法

| 方法 | 含义 |
| ---- | ---- |
| `add(E e)` | 在末尾添加元素 |
| `add(int index, E element)` | 在指定索引位置添加元素 |
| `get(int index)` | 根据索引获取元素 |
| `set(int index, E element)` | 修改指定索引位置的元素 |
| `remove(int index)` | 删除指定索引位置的元素 |
| `remove(Object o)` | 删除指定元素 |
| `size()` | 获取元素个数 |

示例：

```java
import java.util.ArrayList;

public class ListMethodDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Cindy");
        names.add(1, "Bob");

        System.out.println(names);

        names.set(0, "Amy");
        System.out.println(names.get(0));

        names.remove(2);
        System.out.println(names);
    }
}
```

### 6.3 `ArrayList`

`ArrayList` 是最常用的 `List` 实现类。

特点：

1. 底层主要基于数组。
2. 查询元素比较方便。
3. 在末尾添加元素比较常见。
4. 在中间频繁插入和删除元素时，需要移动元素。

常见使用方式：

```java
import java.util.ArrayList;

public class ArrayListDemo {
    public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();

        scores.add(95);
        scores.add(88);
        scores.add(76);

        int total = 0;

        for (int i = 0; i < scores.size(); i++) {
            total += scores.get(i);
        }

        double average = total * 1.0 / scores.size();

        System.out.println(total);
        System.out.println(average);
    }
}
```

### 6.4 `LinkedList`

`LinkedList` 也是 `List` 的实现类。

特点：

1. 可以保存有序、可重复元素。
2. 适合在头部或尾部添加、删除元素。
3. 可以作为队列或双端队列的基础使用。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `addFirst(E e)` | 在开头添加元素 |
| `addLast(E e)` | 在末尾添加元素 |
| `getFirst()` | 获取第一个元素 |
| `getLast()` | 获取最后一个元素 |
| `removeFirst()` | 删除第一个元素 |
| `removeLast()` | 删除最后一个元素 |

示例：

```java
import java.util.LinkedList;

public class LinkedListDemo {
    public static void main(String[] args) {
        LinkedList<String> tasks = new LinkedList<>();

        tasks.addLast("整理资料");
        tasks.addLast("完成作业");
        tasks.addFirst("打开电脑");

        System.out.println(tasks);
        System.out.println(tasks.getFirst());
        System.out.println(tasks.getLast());

        tasks.removeFirst();
        System.out.println(tasks);
    }
}
```

### 6.5 `ArrayList` 与 `LinkedList` 的选择

基础阶段可以按以下方式理解：

1. 大多数情况下优先使用 `ArrayList`。
2. 如果经常在头部或尾部进行添加、删除，可以了解 `LinkedList`。
3. 如果只是顺序保存和遍历一批数据，`ArrayList` 更常见。

示例：

```java
List<String> names = new ArrayList<>();
```

实际开发中，变量类型常写成接口类型 `List`，对象创建时选择具体实现类 `ArrayList`。

```java
import java.util.ArrayList;
import java.util.List;

public class ListReferenceDemo {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");

        System.out.println(names);
    }
}
```

这种写法体现了面向接口编程的思想。

## 7. `Set`

### 7.1 `Set` 的特点

`Set` 是不允许重复元素的集合。

```java
import java.util.HashSet;

public class SetDemo {
    public static void main(String[] args) {
        HashSet<String> names = new HashSet<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Alice");

        System.out.println(names);
        System.out.println(names.size());
    }
}
```

虽然添加了两次 `"Alice"`，但集合中只会保留一个。

`Set` 常用于去重。

### 7.2 `HashSet`

`HashSet` 是常用的 `Set` 实现类。

特点：

1. 元素不重复。
2. 不保证遍历顺序与添加顺序一致。
3. 查找效率通常较高。

示例：

```java
import java.util.HashSet;

public class HashSetDemo {
    public static void main(String[] args) {
        HashSet<Integer> numbers = new HashSet<>();

        numbers.add(3);
        numbers.add(1);
        numbers.add(2);
        numbers.add(3);

        for (Integer number : numbers) {
            System.out.println(number);
        }
    }
}
```

运行时应重点观察：输出顺序不一定等于添加顺序。

### 7.3 `LinkedHashSet`

`LinkedHashSet` 也是不允许重复元素的集合，但它会保留元素的添加顺序。

```java
import java.util.LinkedHashSet;

public class LinkedHashSetDemo {
    public static void main(String[] args) {
        LinkedHashSet<String> names = new LinkedHashSet<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");
        names.add("Alice");

        System.out.println(names);
    }
}
```

输出顺序通常与添加顺序一致，并且重复元素不会被保留。

### 7.4 `TreeSet`

`TreeSet` 可以对元素进行排序。

```java
import java.util.TreeSet;

public class TreeSetDemo {
    public static void main(String[] args) {
        TreeSet<Integer> numbers = new TreeSet<>();

        numbers.add(30);
        numbers.add(10);
        numbers.add(20);
        numbers.add(10);

        System.out.println(numbers);
    }
}
```

输出结果会按从小到大排列，并去除重复元素。

`TreeSet` 中保存的元素需要能够比较大小。对于 `Integer`、`Double`、`String` 这类常见类型，Java 已经提供了自然排序规则。

### 7.5 自定义对象去重

当 `HashSet` 保存自定义对象时，如果希望按照对象内容去重，通常需要重写 `equals` 和 `hashCode` 方法。

```java
public class Student {
    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Student)) {
            return false;
        }

        Student other = (Student) obj;
        return age == other.age && name.equals(other.name);
    }

    @Override
    public int hashCode() {
        return name.hashCode() * 31 + age;
    }

    @Override
    public String toString() {
        return name + "-" + age;
    }
}
```

使用示例：

```java
import java.util.HashSet;

public class StudentSetDemo {
    public static void main(String[] args) {
        HashSet<Student> students = new HashSet<>();

        students.add(new Student("Alice", 18));
        students.add(new Student("Bob", 19));
        students.add(new Student("Alice", 18));

        System.out.println(students);
    }
}
```

运行后应观察：两个姓名和年龄都相同的学生对象会被认为是重复元素。

## 8. `Map`

### 8.1 什么是 `Map`

`Map` 用键值对保存数据。

```text
key -> value
```

示例：

```text
"Alice" -> 95
"Bob" -> 88
"Cindy" -> 76
```

`Map` 的特点：

1. 一个键对应一个值。
2. 键不能重复。
3. 值可以重复。
4. 根据键可以快速查找对应值。

### 8.2 `HashMap`

`HashMap` 是最常用的 `Map` 实现类。

```java
import java.util.HashMap;

public class HashMapDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();

        scores.put("Alice", 95);
        scores.put("Bob", 88);
        scores.put("Cindy", 76);

        System.out.println(scores.get("Alice"));
        System.out.println(scores.containsKey("Bob"));
        System.out.println(scores.size());
    }
}
```

其中：

1. `String` 是键的类型。
2. `Integer` 是值的类型。
3. `put` 用于添加或修改键值对。
4. `get` 用于根据键获取值。

### 8.3 `Map` 常用方法

| 方法 | 含义 |
| ---- | ---- |
| `put(K key, V value)` | 添加或修改键值对 |
| `get(Object key)` | 根据键获取值 |
| `remove(Object key)` | 根据键删除键值对 |
| `containsKey(Object key)` | 判断是否包含指定键 |
| `containsValue(Object value)` | 判断是否包含指定值 |
| `size()` | 获取键值对数量 |
| `isEmpty()` | 判断是否为空 |
| `keySet()` | 获取所有键 |
| `values()` | 获取所有值 |
| `entrySet()` | 获取所有键值对 |

示例：

```java
import java.util.HashMap;

public class MapMethodDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();

        scores.put("Alice", 95);
        scores.put("Bob", 88);
        scores.put("Cindy", 76);
        scores.put("Bob", 90);

        System.out.println(scores);
        System.out.println(scores.get("Bob"));

        scores.remove("Cindy");

        System.out.println(scores.containsKey("Cindy"));
        System.out.println(scores.size());
    }
}
```

注意：当 `put` 使用已经存在的键时，会用新值覆盖旧值。

### 8.4 遍历 `Map` 的键

可以先通过 `keySet()` 获取所有键，再根据键获取值。

```java
import java.util.HashMap;
import java.util.Set;

public class MapKeySetDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();

        scores.put("Alice", 95);
        scores.put("Bob", 88);
        scores.put("Cindy", 76);

        Set<String> names = scores.keySet();

        for (String name : names) {
            Integer score = scores.get(name);
            System.out.println(name + " -> " + score);
        }
    }
}
```

### 8.5 遍历 `Map` 的键值对

也可以使用 `entrySet()` 获取所有键值对。

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Set;

public class MapEntryDemo {
    public static void main(String[] args) {
        HashMap<String, Integer> scores = new HashMap<>();

        scores.put("Alice", 95);
        scores.put("Bob", 88);
        scores.put("Cindy", 76);

        Set<Map.Entry<String, Integer>> entries = scores.entrySet();

        for (Map.Entry<String, Integer> entry : entries) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

`entrySet()` 遍历适合同时使用键和值的场景。

### 8.6 `LinkedHashMap`

`LinkedHashMap` 会按照添加顺序保存键值对。

```java
import java.util.LinkedHashMap;

public class LinkedHashMapDemo {
    public static void main(String[] args) {
        LinkedHashMap<String, Integer> scores = new LinkedHashMap<>();

        scores.put("Alice", 95);
        scores.put("Bob", 88);
        scores.put("Cindy", 76);

        System.out.println(scores);
    }
}
```

当需要让遍历顺序与添加顺序一致时，可以使用 `LinkedHashMap`。

### 8.7 `TreeMap`

`TreeMap` 会根据键进行排序。

```java
import java.util.TreeMap;

public class TreeMapDemo {
    public static void main(String[] args) {
        TreeMap<String, Integer> scores = new TreeMap<>();

        scores.put("Bob", 88);
        scores.put("Cindy", 76);
        scores.put("Alice", 95);

        System.out.println(scores);
    }
}
```

输出中键会按照自然顺序排列。

## 9. 集合遍历

### 9.1 普通 `for` 循环

普通 `for` 循环适合遍历有索引的 `List`。

```java
import java.util.ArrayList;

public class ForListDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        for (int i = 0; i < names.size(); i++) {
            System.out.println(names.get(i));
        }
    }
}
```

`Set` 没有索引，不能使用 `get(i)` 的方式遍历。

### 9.2 增强 `for` 循环

增强 `for` 循环适合遍历数组和集合。

```java
import java.util.ArrayList;

public class EnhancedForDemo {
    public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();

        scores.add(95);
        scores.add(88);
        scores.add(76);

        for (Integer score : scores) {
            System.out.println(score);
        }
    }
}
```

增强 `for` 循环强调读取元素，适合只查看元素内容的场景。

### 9.3 `Iterator`

`Iterator` 是迭代器，用于逐个访问集合元素。

```java
import java.util.ArrayList;
import java.util.Iterator;

public class IteratorDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        Iterator<String> iterator = names.iterator();

        while (iterator.hasNext()) {
            String name = iterator.next();
            System.out.println(name);
        }
    }
}
```

其中：

1. `iterator()` 获取迭代器。
2. `hasNext()` 判断是否还有下一个元素。
3. `next()` 获取下一个元素。

### 9.4 `forEach` 方法

集合也可以使用 `forEach` 方法遍历。

```java
import java.util.ArrayList;

public class ForEachMethodDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        names.forEach(name -> System.out.println(name));
    }
}
```

`name -> System.out.println(name)` 是 Lambda 表达式。基础阶段只需要知道它表示“对集合中的每个元素执行一次输出操作”。

更简洁的写法：

```java
names.forEach(System.out::println);
```

本任务只要求能看懂和简单使用 `forEach`，不要求深入学习 Lambda 表达式。

## 10. 集合排序与比较

### 10.1 `Collections.sort`

`Collections.sort` 可以对 `List` 进行排序。

```java
import java.util.ArrayList;
import java.util.Collections;

public class SortDemo {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();

        numbers.add(30);
        numbers.add(10);
        numbers.add(20);

        Collections.sort(numbers);

        System.out.println(numbers);
    }
}
```

`String` 也可以按自然顺序排序。

```java
import java.util.ArrayList;
import java.util.Collections;

public class StringSortDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Bob");
        names.add("Cindy");
        names.add("Alice");

        Collections.sort(names);

        System.out.println(names);
    }
}
```

### 10.2 `Comparable`

如果要让自定义对象具备自然排序能力，可以实现 `Comparable` 接口。

```java
public class Product implements Comparable<Product> {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public int compareTo(Product other) {
        return Double.compare(this.price, other.price);
    }

    @Override
    public String toString() {
        return name + ":" + price;
    }
}
```

使用示例：

```java
import java.util.ArrayList;
import java.util.Collections;

public class ProductSortDemo {
    public static void main(String[] args) {
        ArrayList<Product> products = new ArrayList<>();

        products.add(new Product("Book", 39.9));
        products.add(new Product("Pen", 5.5));
        products.add(new Product("Bag", 99.0));

        Collections.sort(products);

        System.out.println(products);
    }
}
```

`compareTo` 方法返回值含义：

1. 返回负数：当前对象排在前面。
2. 返回 0：两个对象排序位置相同。
3. 返回正数：当前对象排在后面。

### 10.3 `Comparator`

`Comparator` 可以在不修改类本身的情况下指定排序规则。

```java
import java.util.ArrayList;
import java.util.Comparator;

public class ComparatorDemo {
    public static void main(String[] args) {
        ArrayList<Product> products = new ArrayList<>();

        products.add(new Product("Book", 39.9));
        products.add(new Product("Pen", 5.5));
        products.add(new Product("Bag", 99.0));

        products.sort(new Comparator<Product>() {
            @Override
            public int compare(Product a, Product b) {
                return Double.compare(b.getPrice(), a.getPrice());
            }
        });

        System.out.println(products);
    }
}
```

上述代码按照价格从高到低排序。

也可以使用 Lambda 表达式简化：

```java
products.sort((a, b) -> Double.compare(b.getPrice(), a.getPrice()));
```

基础阶段重点理解：`Comparable` 通常表示类自己的默认排序规则，`Comparator` 表示临时传入的排序规则。

## 11. `Collections` 工具类

### 11.1 `Collections` 的含义

`Collections` 是操作集合的工具类，里面提供了很多静态方法。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `sort(List<T> list)` | 对列表排序 |
| `reverse(List<?> list)` | 反转列表 |
| `shuffle(List<?> list)` | 打乱列表 |
| `max(Collection<T> coll)` | 获取最大值 |
| `min(Collection<T> coll)` | 获取最小值 |
| `addAll(Collection<T> c, T... elements)` | 一次添加多个元素 |
| `frequency(Collection<?> c, Object o)` | 统计元素出现次数 |

### 11.2 常用方法示例

```java
import java.util.ArrayList;
import java.util.Collections;

public class CollectionsDemo {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();

        Collections.addAll(numbers, 30, 10, 20, 10);

        System.out.println(numbers);
        System.out.println(Collections.max(numbers));
        System.out.println(Collections.min(numbers));
        System.out.println(Collections.frequency(numbers, 10));

        Collections.sort(numbers);
        System.out.println(numbers);

        Collections.reverse(numbers);
        System.out.println(numbers);
    }
}
```

`Collections` 只能直接操作集合，不能直接操作普通数组。普通数组通常使用 `Arrays` 工具类。

## 12. `Arrays` 工具类

### 12.1 `Arrays` 的含义

`Arrays` 是操作数组的工具类。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `toString(array)` | 将数组内容转换为字符串 |
| `sort(array)` | 对数组排序 |
| `binarySearch(array, value)` | 二分查找 |
| `copyOf(array, newLength)` | 复制数组 |
| `equals(array1, array2)` | 比较两个数组内容 |
| `asList(elements)` | 将多个元素转换为列表 |

### 12.2 常用方法示例

```java
import java.util.Arrays;

public class ArraysDemo {
    public static void main(String[] args) {
        int[] numbers = {30, 10, 20};

        System.out.println(Arrays.toString(numbers));

        Arrays.sort(numbers);
        System.out.println(Arrays.toString(numbers));

        int index = Arrays.binarySearch(numbers, 20);
        System.out.println(index);

        int[] copied = Arrays.copyOf(numbers, 5);
        System.out.println(Arrays.toString(copied));
    }
}
```

注意：`binarySearch` 通常要求数组已经排序，否则结果没有可靠意义。

### 12.3 `Arrays.asList`

`Arrays.asList` 可以快速创建一个列表。

```java
import java.util.Arrays;
import java.util.List;

public class AsListDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Cindy");

        System.out.println(names);
        System.out.println(names.get(0));
    }
}
```

基础阶段建议把 `Arrays.asList` 主要看作一种快速创建固定内容列表的方式。需要频繁添加或删除元素时，可以再创建新的 `ArrayList`。

```java
import java.util.ArrayList;
import java.util.Arrays;

public class MutableListDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>(Arrays.asList("Alice", "Bob"));

        names.add("Cindy");

        System.out.println(names);
    }
}
```

## 13. `Objects` 工具类

### 13.1 `Objects` 的含义

`Objects` 是操作对象的工具类，常用于对象比较、对象转字符串、生成哈希值等。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `Objects.equals(a, b)` | 比较两个对象是否相等 |
| `Objects.toString(obj)` | 将对象转换为字符串 |
| `Objects.hash(values...)` | 根据多个值生成哈希值 |
| `Objects.isNull(obj)` | 判断对象是否为 `null` |
| `Objects.nonNull(obj)` | 判断对象是否不为 `null` |

### 13.2 `Objects.equals`

`Objects.equals` 可以比较两个对象内容是否相等。

```java
import java.util.Objects;

public class ObjectsEqualsDemo {
    public static void main(String[] args) {
        String a = "Java";
        String b = new String("Java");
        String c = null;

        System.out.println(Objects.equals(a, b));
        System.out.println(Objects.equals(a, c));
        System.out.println(Objects.equals(c, null));
    }
}
```

与直接调用 `a.equals(b)` 相比，`Objects.equals(a, b)` 的写法在比较对象时更统一，也能处理两个引用中存在 `null` 的情况。

本任务只要求知道 `Objects.equals` 是一种常见对象比较工具方法。

### 13.3 在自定义类中使用 `Objects`

```java
import java.util.Objects;

public class Book {
    private String title;
    private String author;

    public Book(String title, String author) {
        this.title = title;
        this.author = author;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Book)) {
            return false;
        }

        Book other = (Book) obj;
        return Objects.equals(title, other.title)
                && Objects.equals(author, other.author);
    }

    @Override
    public int hashCode() {
        return Objects.hash(title, author);
    }
}
```

`Objects.equals` 和 `Objects.hash` 可以让 `equals`、`hashCode` 的代码更简洁。

## 14. `Math` 工具类

### 14.1 `Math` 的含义

`Math` 是数学工具类，提供常见数学运算方法。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `Math.abs(value)` | 绝对值 |
| `Math.max(a, b)` | 较大值 |
| `Math.min(a, b)` | 较小值 |
| `Math.pow(a, b)` | 幂运算 |
| `Math.sqrt(value)` | 平方根 |
| `Math.round(value)` | 四舍五入 |
| `Math.ceil(value)` | 向上取整 |
| `Math.floor(value)` | 向下取整 |
| `Math.random()` | 生成 `[0.0, 1.0)` 范围随机小数 |

### 14.2 常用方法示例

```java
public class MathDemo {
    public static void main(String[] args) {
        System.out.println(Math.abs(-10));
        System.out.println(Math.max(10, 20));
        System.out.println(Math.min(10, 20));
        System.out.println(Math.pow(2, 3));
        System.out.println(Math.sqrt(16));
        System.out.println(Math.round(3.6));
        System.out.println(Math.ceil(3.1));
        System.out.println(Math.floor(3.9));
    }
}
```

### 14.3 使用 `Math.random`

`Math.random()` 返回一个大于等于 `0.0` 且小于 `1.0` 的随机小数。

生成 `1` 到 `10` 的随机整数：

```java
public class MathRandomDemo {
    public static void main(String[] args) {
        int number = (int) (Math.random() * 10) + 1;
        System.out.println(number);
    }
}
```

其中：

1. `Math.random() * 10` 得到 `[0.0, 10.0)` 范围的小数。
2. `(int)` 强制转换后得到 `0` 到 `9`。
3. 再加 `1` 得到 `1` 到 `10`。

## 15. `Random` 工具类

### 15.1 `Random` 的含义

`Random` 用于生成随机数。

```java
import java.util.Random;

public class RandomDemo {
    public static void main(String[] args) {
        Random random = new Random();

        int number = random.nextInt(10);
        double value = random.nextDouble();
        boolean flag = random.nextBoolean();

        System.out.println(number);
        System.out.println(value);
        System.out.println(flag);
    }
}
```

其中：

1. `nextInt(10)` 生成 `0` 到 `9` 的随机整数。
2. `nextDouble()` 生成 `[0.0, 1.0)` 范围随机小数。
3. `nextBoolean()` 生成随机布尔值。

### 15.2 生成指定范围随机整数

生成 `min` 到 `max` 的随机整数：

```java
import java.util.Random;

public class RandomRangeDemo {
    public static void main(String[] args) {
        Random random = new Random();

        int min = 1;
        int max = 100;

        int number = random.nextInt(max - min + 1) + min;

        System.out.println(number);
    }
}
```

公式：

```text
random.nextInt(max - min + 1) + min
```

其中 `min` 和 `max` 都可能被生成。

## 16. `StringBuilder` 与 `StringJoiner`

### 16.1 `StringBuilder`

`StringBuilder` 用于高效拼接字符串。

在循环中频繁使用 `+` 拼接字符串时，会创建较多中间字符串。`StringBuilder` 可以用可变对象保存拼接过程。

```java
public class StringBuilderDemo {
    public static void main(String[] args) {
        StringBuilder builder = new StringBuilder();

        builder.append("Java");
        builder.append(" ");
        builder.append("Collection");

        String result = builder.toString();

        System.out.println(result);
    }
}
```

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `append(value)` | 追加内容 |
| `insert(index, value)` | 在指定位置插入内容 |
| `delete(start, end)` | 删除指定范围内容 |
| `reverse()` | 反转内容 |
| `toString()` | 转换为 `String` |

### 16.2 使用 `StringBuilder` 拼接集合内容

```java
import java.util.ArrayList;

public class BuilderListDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();

        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        StringBuilder builder = new StringBuilder();

        for (int i = 0; i < names.size(); i++) {
            builder.append(names.get(i));

            if (i < names.size() - 1) {
                builder.append(", ");
            }
        }

        System.out.println(builder.toString());
    }
}
```

### 16.3 `StringJoiner`

`StringJoiner` 用于按照指定分隔符拼接字符串。

```java
import java.util.StringJoiner;

public class StringJoinerDemo {
    public static void main(String[] args) {
        StringJoiner joiner = new StringJoiner(", ");

        joiner.add("Alice");
        joiner.add("Bob");
        joiner.add("Cindy");

        System.out.println(joiner.toString());
    }
}
```

也可以指定前缀和后缀。

```java
import java.util.StringJoiner;

public class StringJoinerPrefixDemo {
    public static void main(String[] args) {
        StringJoiner joiner = new StringJoiner(", ", "[", "]");

        joiner.add("Alice");
        joiner.add("Bob");
        joiner.add("Cindy");

        System.out.println(joiner.toString());
    }
}
```

输出形式类似：

```text
[Alice, Bob, Cindy]
```

## 17. `BigDecimal`

### 17.1 为什么需要 `BigDecimal`

`double` 和 `float` 适合表示浮点数，但它们在某些小数计算中可能出现精度问题。

```java
public class DoublePrecisionDemo {
    public static void main(String[] args) {
        System.out.println(0.1 + 0.2);
    }
}
```

运行结果可能不是精确的 `0.3`。

在需要精确小数计算的场景中，可以使用 `BigDecimal`。

### 17.2 创建 `BigDecimal`

建议使用字符串创建 `BigDecimal`。

```java
import java.math.BigDecimal;

public class BigDecimalCreateDemo {
    public static void main(String[] args) {
        BigDecimal a = new BigDecimal("0.1");
        BigDecimal b = new BigDecimal("0.2");

        System.out.println(a);
        System.out.println(b);
    }
}
```

### 17.3 `BigDecimal` 常用运算

```java
import java.math.BigDecimal;

public class BigDecimalDemo {
    public static void main(String[] args) {
        BigDecimal a = new BigDecimal("10.5");
        BigDecimal b = new BigDecimal("2.0");

        System.out.println(a.add(b));
        System.out.println(a.subtract(b));
        System.out.println(a.multiply(b));
        System.out.println(a.divide(b));
    }
}
```

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `add` | 加法 |
| `subtract` | 减法 |
| `multiply` | 乘法 |
| `divide` | 除法 |
| `compareTo` | 比较大小 |

注意：本任务不展开讲解除法无法整除时的处理方式。练习中只使用可以整除的小数。

### 17.4 `BigDecimal` 比较

`BigDecimal` 比较大小通常使用 `compareTo`。

```java
import java.math.BigDecimal;

public class BigDecimalCompareDemo {
    public static void main(String[] args) {
        BigDecimal a = new BigDecimal("10.0");
        BigDecimal b = new BigDecimal("10.00");

        System.out.println(a.equals(b));
        System.out.println(a.compareTo(b) == 0);
    }
}
```

其中：

1. `equals` 会比较数值和精度。
2. `compareTo` 主要比较数值大小。

在数值大小比较时，更常使用 `compareTo`。

## 18. 代码示例

在 IDEA 中创建以下文件，并保证它们位于同一个包或同一个源码目录下。

### 18.1 `Course.java`

```java
import java.math.BigDecimal;
import java.util.Objects;

public class Course implements Comparable<Course> {
    private String name;
    private int credit;
    private BigDecimal price;

    public Course(String name, int credit, BigDecimal price) {
        this.name = name;
        this.credit = credit;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public int getCredit() {
        return credit;
    }

    public BigDecimal getPrice() {
        return price;
    }

    @Override
    public int compareTo(Course other) {
        return Integer.compare(this.credit, other.credit);
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true;
        }

        if (!(obj instanceof Course)) {
            return false;
        }

        Course other = (Course) obj;
        return Objects.equals(name, other.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name);
    }

    @Override
    public String toString() {
        return name + "(" + credit + "学分, " + price + "元)";
    }
}
```

### 18.2 `Box.java`

```java
public class Box<T> {
    private T value;

    public Box(T value) {
        this.value = value;
    }

    public T getValue() {
        return value;
    }

    public void setValue(T value) {
        this.value = value;
    }
}
```

### 18.3 `Task4Demo.java`

```java
import java.math.BigDecimal;
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.HashSet;
import java.util.Map;
import java.util.Random;
import java.util.StringJoiner;

public class Task4Demo {
    public static <T> void printBox(Box<T> box) {
        System.out.println(box.getValue());
    }

    public static void main(String[] args) {
        Box<String> titleBox = new Box<>("Task 4");
        Box<Integer> scoreBox = new Box<>(100);

        printBox(titleBox);
        printBox(scoreBox);

        ArrayList<Course> courses = new ArrayList<>();
        courses.add(new Course("Java 基础", 4, new BigDecimal("399.00")));
        courses.add(new Course("数据库基础", 3, new BigDecimal("299.00")));
        courses.add(new Course("前端基础", 2, new BigDecimal("199.00")));

        Collections.sort(courses);
        System.out.println(courses);

        courses.sort((a, b) -> b.getPrice().compareTo(a.getPrice()));
        System.out.println(courses);

        HashSet<Course> courseSet = new HashSet<>(courses);
        courseSet.add(new Course("Java 基础", 4, new BigDecimal("399.00")));
        System.out.println(courseSet.size());

        HashMap<String, Integer> selectedCount = new HashMap<>();
        selectedCount.put("Java 基础", 32);
        selectedCount.put("数据库基础", 20);
        selectedCount.put("前端基础", 25);

        for (Map.Entry<String, Integer> entry : selectedCount.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }

        StringJoiner joiner = new StringJoiner(", ", "[", "]");

        for (Course course : courses) {
            joiner.add(course.getName());
        }

        System.out.println(joiner.toString());

        Random random = new Random();
        int luckyNumber = random.nextInt(100) + 1;
        System.out.println(luckyNumber);
    }
}
```

运行后应重点观察以下内容：

1. `Box<T>` 如何通过泛型保存不同类型的数据。
2. `ArrayList<Course>` 如何保存课程对象。
3. `Collections.sort(courses)` 如何使用 `Course` 的自然排序规则。
4. `courses.sort(...)` 如何使用临时比较器改变排序规则。
5. `HashSet<Course>` 如何结合 `equals` 和 `hashCode` 完成去重。
6. `HashMap<String, Integer>` 如何保存课程名称和选课人数。
7. `StringJoiner` 如何拼接课程名称。
8. `Random` 如何生成随机整数。
9. `BigDecimal` 如何表示课程价格。

## 19. 练习

### 19.1 包装类练习

创建 `WrapperPractice.java`，完成以下要求：

1. 声明并初始化 `Integer`、`Double`、`Character`、`Boolean` 类型变量。
2. 使用自动装箱创建包装类对象。
3. 使用自动拆箱让 `Integer` 参与加法运算。
4. 使用 `Integer.parseInt` 将 `"100"` 转换为 `int`。
5. 使用 `Double.parseDouble` 将 `"19.9"` 转换为 `double`。
6. 使用 `Integer.MAX_VALUE` 输出 `int` 的最大值。
7. 创建两个值相同的 `Integer` 对象，分别使用 `==` 和 `equals` 比较，并用注释说明区别。

### 19.2 泛型练习

创建 `GenericPractice.java`，完成以下要求：

1. 定义泛型类 `Pair<K, V>`。
2. 在 `Pair` 中定义 `key` 和 `value` 两个成员变量。
3. 为 `Pair` 定义构造方法、`getKey` 和 `getValue` 方法。
4. 创建 `Pair<String, Integer>` 对象表示学生姓名和成绩。
5. 创建 `Pair<String, String>` 对象表示用户名和邮箱。
6. 定义泛型方法 `printArray`，可以输出任意引用类型数组。
7. 使用 `String[]` 和 `Integer[]` 测试 `printArray` 方法。
8. 定义方法 `sumNumbers`，参数类型为 `ArrayList<? extends Number>`，返回所有数字的总和。

### 19.3 `List` 练习

创建 `ListPractice.java`，完成以下要求：

1. 创建 `ArrayList<String>` 保存至少 5 个学生姓名。
2. 使用 `add` 添加元素。
3. 使用 `add(index, element)` 在指定位置插入元素。
4. 使用 `set` 修改一个元素。
5. 使用 `remove` 删除一个元素。
6. 使用普通 `for` 循环遍历并输出所有元素。
7. 使用增强 `for` 循环再次遍历所有元素。
8. 创建 `LinkedList<String>` 保存待办事项。
9. 使用 `addFirst`、`addLast`、`removeFirst`、`getLast` 测试头尾操作。

### 19.4 `Set` 练习

创建 `SetPractice.java`，完成以下要求：

1. 创建 `HashSet<String>` 保存若干城市名称，其中包含重复名称。
2. 输出集合内容和元素数量，观察去重效果。
3. 创建 `LinkedHashSet<String>` 保存同样的数据，观察添加顺序。
4. 创建 `TreeSet<Integer>` 保存若干整数，观察排序效果。
5. 定义 `Student` 类，包含 `name` 和 `age`。
6. 在 `Student` 中重写 `equals`、`hashCode` 和 `toString`。
7. 创建 `HashSet<Student>`，添加内容相同的学生对象，观察去重效果。

### 19.5 `Map` 练习

创建 `MapPractice.java`，完成以下要求：

1. 创建 `HashMap<String, Integer>` 保存学生姓名和成绩。
2. 使用 `put` 添加至少 5 条数据。
3. 使用相同键再次 `put`，观察值覆盖效果。
4. 使用 `get` 根据姓名获取成绩。
5. 使用 `containsKey` 判断某个学生是否存在。
6. 使用 `keySet` 遍历所有键和值。
7. 使用 `entrySet` 遍历所有键值对。
8. 创建 `LinkedHashMap<String, Integer>` 观察添加顺序。
9. 创建 `TreeMap<String, Integer>` 观察按键排序效果。

### 19.6 工具类练习

创建 `UtilityPractice.java`，完成以下要求：

1. 创建 `int[] numbers = {30, 10, 20, 40};`。
2. 使用 `Arrays.toString` 输出数组内容。
3. 使用 `Arrays.sort` 对数组排序。
4. 使用 `Arrays.binarySearch` 查找元素。
5. 使用 `Arrays.copyOf` 复制数组并扩容。
6. 创建 `ArrayList<Integer>`，使用 `Collections.addAll` 添加多个整数。
7. 使用 `Collections.max`、`Collections.min`、`Collections.frequency`。
8. 使用 `Collections.sort` 和 `Collections.reverse`。
9. 使用 `Math.max`、`Math.sqrt`、`Math.round`。
10. 使用 `Random` 生成 `1` 到 `50` 的随机整数。
11. 使用 `StringBuilder` 拼接多个学生姓名。
12. 使用 `StringJoiner` 生成类似 `[Alice, Bob, Cindy]` 的字符串。
13. 使用 `Objects.equals` 比较两个字符串。
14. 使用 `BigDecimal` 完成两个价格的加法、减法和乘法。

### 19.7 综合练习

创建 `Task4Practice.java`，完成以下要求：

1. 定义 `Product` 类，包含 `name`、`category`、`price`、`stock`。
2. `price` 使用 `BigDecimal` 类型。
3. 在 `Product` 中定义构造方法、getter 方法和 `toString` 方法。
4. 根据 `name` 重写 `equals` 和 `hashCode`。
5. 让 `Product` 实现 `Comparable<Product>`，默认按库存从低到高排序。
6. 创建 `ArrayList<Product>` 保存至少 5 个商品。
7. 使用 `Collections.sort` 按库存排序。
8. 使用 `Comparator` 或 Lambda 表达式按价格从高到低排序。
9. 使用 `HashSet<Product>` 对商品去重。
10. 使用 `HashMap<String, ArrayList<Product>>` 按商品分类保存商品列表。
11. 遍历 `Map`，输出每个分类下的商品。
12. 使用 `StringJoiner` 拼接所有商品名称。
13. 使用 `BigDecimal` 计算所有商品的总价格。
14. 使用 `Random` 随机选出一个商品作为推荐商品。
15. 使用注释说明本程序中哪里使用了泛型、包装类、集合和工具类。

## 20. 验收标准

满足以下条件视为完成 Task 4：

1. 能说明基本类型和包装类之间的对应关系。
2. 能说明自动装箱和自动拆箱的含义。
3. 能使用包装类完成字符串到基本类型的转换。
4. 能说明泛型的作用。
5. 能正确创建带泛型的集合对象。
6. 能说明泛型为什么不能直接使用基本类型。
7. 能定义简单泛型类和泛型方法。
8. 能看懂泛型通配符和泛型上限的基本写法。
9. 能说明数组和集合的区别。
10. 能说明 `List`、`Set`、`Map` 的主要区别。
11. 能使用 `ArrayList` 完成添加、修改、删除、查询和遍历。
12. 能使用 `LinkedList` 完成头尾操作。
13. 能说明 `HashSet`、`LinkedHashSet`、`TreeSet` 的区别。
14. 能通过重写 `equals` 和 `hashCode` 让自定义对象在 `HashSet` 中按内容去重。
15. 能使用 `HashMap` 保存和遍历键值对。
16. 能说明 `HashMap`、`LinkedHashMap`、`TreeMap` 的区别。
17. 能使用普通 `for`、增强 `for`、`Iterator` 和 `forEach` 遍历集合。
18. 能使用 `Comparable` 或 `Comparator` 对自定义对象排序。
19. 能使用 `Collections` 操作集合。
20. 能使用 `Arrays` 操作数组。
21. 能使用 `Objects.equals` 比较对象。
22. 能使用 `Math` 和 `Random` 完成基础数学与随机数操作。
23. 能使用 `StringBuilder` 或 `StringJoiner` 拼接字符串。
24. 能使用 `BigDecimal` 完成基础小数计算。
25. 能独立完成 `Task4Practice.java` 并运行得到正确结果。

## 21. 提交规范

1. 学习/实验总结报告统一命名为 `Task_4_report.md`，放置在 `Task_4/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_4/src/` 目录下（或在 `Task_4/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_4/` 目录中的本阶段内容。
