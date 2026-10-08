# Task 6：Java 枚举、正则、日期时间、Lambda、Stream 与多线程

## 1. 任务说明

本任务补充 Java 基础学习中非常常用的一组能力，重点包括枚举、正则表达式、日期时间 API、Lambda 表达式、函数式接口、Stream API，以及多线程基础。

前面的任务已经学习了面向对象、集合、异常和 IO。进入本任务后，程序不再只是“保存数据、遍历数据、读写文件”，还会开始处理更真实的开发场景：

1. 使用枚举表示固定范围的业务状态。
2. 使用正则表达式校验和提取文本。
3. 使用日期时间 API 表示时间、格式化时间、计算时间差。
4. 使用 Lambda 简化匿名内部类和集合操作。
5. 使用 Stream 更方便地筛选、转换、排序和统计集合数据。
6. 使用多线程让程序同时执行多个任务。

本任务内容较多，应重点理解每种技术解决什么问题，而不是只背语法。

## 2. 知识范围

本任务包含以下内容：

1. 枚举的含义。
2. 枚举常量、成员变量、构造方法和成员方法。
3. 枚举常用方法。
4. 枚举与 `switch`。
5. 正则表达式基础。
6. `String` 中的正则相关方法。
7. `Pattern` 和 `Matcher`。
8. 日期时间 API。
9. `LocalDate`、`LocalTime`、`LocalDateTime`。
10. `DateTimeFormatter`。
11. `Duration` 和 `Period`。
12. Lambda 表达式。
13. 函数式接口。
14. 常用函数式接口。
15. 方法引用。
16. Stream API。
17. Stream 的创建、中间操作和终结操作。
18. 多线程基础。
19. `Thread` 和 `Runnable`。
20. `Callable`、`Future` 和线程池。
21. 线程安全。
22. `synchronized`。
23. `volatile` 基础。
24. `CompletableFuture` 入门。

## 3. 枚举

### 3.1 什么是枚举

枚举用于表示一组固定的常量。

例如：

1. 一周中的星期。
2. 订单状态。
3. 用户角色。
4. 性别。
5. 支付方式。

如果某个变量只能从固定几个值中选择，就可以考虑使用枚举。

示例：

```java
public enum Gender {
    MALE,
    FEMALE
}
```

使用枚举：

```java
public class EnumDemo {
    public static void main(String[] args) {
        Gender gender = Gender.MALE;
        System.out.println(gender);
    }
}
```

其中：

1. `enum` 用于定义枚举。
2. `MALE` 和 `FEMALE` 是枚举常量。
3. 枚举变量只能保存枚举中定义过的常量。

### 3.2 为什么需要枚举

不使用枚举时，可能用字符串表示状态。

```java
String status = "PAID";
```

这种写法的问题是：字符串很容易写错。

```java
String status = "PAYED";
```

使用枚举可以限制取值范围。

```java
public enum OrderStatus {
    CREATED,
    PAID,
    SHIPPED,
    FINISHED,
    CANCELED
}
```

使用：

```java
OrderStatus status = OrderStatus.PAID;
```

如果写成不存在的枚举常量，编译器会直接报错。

### 3.3 枚举中的成员变量和构造方法

枚举也可以有成员变量和构造方法。

```java
public enum OrderStatus {
    CREATED("已创建"),
    PAID("已支付"),
    SHIPPED("已发货"),
    FINISHED("已完成"),
    CANCELED("已取消");

    private String description;

    OrderStatus(String description) {
        this.description = description;
    }

    public String getDescription() {
        return description;
    }
}
```

其中：

1. 每个枚举常量都可以传入构造参数。
2. 枚举构造方法默认是 `private`，不能写成 `public`。
3. 枚举常量必须写在最前面。
4. 如果枚举后面还有成员变量或方法，枚举常量列表结尾要写分号。

使用：

```java
public class OrderStatusDemo {
    public static void main(String[] args) {
        OrderStatus status = OrderStatus.PAID;
        System.out.println(status.getDescription());
    }
}
```

### 3.4 枚举常用方法

所有枚举都自动继承自 `java.lang.Enum`。

常用方法：

| 方法 | 含义 |
| ---- | ---- |
| `name()` | 获取枚举常量名称 |
| `ordinal()` | 获取枚举常量序号 |
| `values()` | 获取所有枚举常量 |
| `valueOf(String name)` | 根据名称获取枚举常量 |

示例：

```java
public class EnumMethodDemo {
    public static void main(String[] args) {
        OrderStatus status = OrderStatus.PAID;

        System.out.println(status.name());
        System.out.println(status.ordinal());

        for (OrderStatus value : OrderStatus.values()) {
            System.out.println(value + " - " + value.getDescription());
        }

        OrderStatus parsed = OrderStatus.valueOf("SHIPPED");
        System.out.println(parsed.getDescription());
    }
}
```

注意：`ordinal()` 依赖枚举常量声明顺序。业务代码中通常不要依赖序号表示业务含义。

### 3.5 枚举与 `switch`

枚举可以配合 `switch` 使用。

```java
public class EnumSwitchDemo {
    public static void main(String[] args) {
        OrderStatus status = OrderStatus.PAID;

        switch (status) {
            case CREATED:
                System.out.println("等待支付");
                break;
            case PAID:
                System.out.println("准备发货");
                break;
            case SHIPPED:
                System.out.println("运输中");
                break;
            case FINISHED:
                System.out.println("订单完成");
                break;
            case CANCELED:
                System.out.println("订单取消");
                break;
            default:
                System.out.println("未知状态");
        }
    }
}
```

在 `case` 后面直接写枚举常量名，不需要写 `OrderStatus.PAID`。

## 4. 正则表达式

### 4.1 什么是正则表达式

正则表达式是一种用于描述字符串规则的表达式。

它常用于：

1. 判断字符串是否符合格式。
2. 查找字符串中的特定内容。
3. 替换字符串中的内容。
4. 拆分字符串。
5. 提取数据。

示例：

```java
public class RegexIntroDemo {
    public static void main(String[] args) {
        String phone = "13812345678";
        boolean match = phone.matches("1[3-9]\\d{9}");

        System.out.println(match);
    }
}
```

`1[3-9]\\d{9}` 表示：

1. 以 `1` 开头。
2. 第二位是 `3` 到 `9`。
3. 后面还有 9 个数字。

### 4.2 常见正则符号

| 符号 | 含义 |
| ---- | ---- |
| `.` | 任意一个字符 |
| `\\d` | 数字 |
| `\\D` | 非数字 |
| `\\w` | 字母、数字、下划线 |
| `\\W` | 非字母、数字、下划线 |
| `\\s` | 空白字符 |
| `\\S` | 非空白字符 |
| `[abc]` | `a`、`b`、`c` 中任意一个 |
| `[^abc]` | 不是 `a`、`b`、`c` 的字符 |
| `[a-z]` | 小写字母范围 |
| `[A-Z]` | 大写字母范围 |
| `[0-9]` | 数字范围 |

在 Java 字符串中，反斜杠需要转义。因此正则中的 `\d` 在 Java 字符串中要写成 `"\\d"`。

### 4.3 数量词

| 数量词 | 含义 |
| ------ | ---- |
| `?` | 0 次或 1 次 |
| `*` | 0 次或多次 |
| `+` | 1 次或多次 |
| `{n}` | 正好 n 次 |
| `{n,}` | 至少 n 次 |
| `{n,m}` | n 到 m 次 |

示例：

```java
public class RegexQuantifierDemo {
    public static void main(String[] args) {
        System.out.println("123".matches("\\d+"));
        System.out.println("abc".matches("[a-z]{3}"));
        System.out.println("A12".matches("[A-Z]\\d{2}"));
    }
}
```

### 4.4 `String` 中的正则方法

`String` 中有一些方法支持正则表达式。

| 方法 | 含义 |
| ---- | ---- |
| `matches(regex)` | 判断整个字符串是否匹配正则 |
| `replaceAll(regex, replacement)` | 替换所有匹配内容 |
| `split(regex)` | 按正则拆分字符串 |

示例：

```java
import java.util.Arrays;

public class StringRegexDemo {
    public static void main(String[] args) {
        String text = "Java123Python456Go";

        System.out.println(text.matches("[A-Za-z0-9]+"));
        System.out.println(text.replaceAll("\\d+", "#"));

        String[] words = text.split("\\d+");
        System.out.println(Arrays.toString(words));
    }
}
```

### 4.5 `Pattern` 和 `Matcher`

如果需要查找字符串中的多个匹配内容，可以使用 `Pattern` 和 `Matcher`。

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class PatternMatcherDemo {
    public static void main(String[] args) {
        String text = "Alice:95, Bob:88, Cindy:76";

        Pattern pattern = Pattern.compile("[A-Za-z]+:\\d+");
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            System.out.println(matcher.group());
        }
    }
}
```

其中：

1. `Pattern.compile(...)` 编译正则表达式。
2. `pattern.matcher(text)` 创建匹配器。
3. `find()` 查找下一个匹配内容。
4. `group()` 获取当前匹配内容。

### 4.6 分组

小括号可以表示分组。

```java
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class RegexGroupDemo {
    public static void main(String[] args) {
        String text = "Alice:95";

        Pattern pattern = Pattern.compile("([A-Za-z]+):(\\d+)");
        Matcher matcher = pattern.matcher(text);

        if (matcher.find()) {
            System.out.println(matcher.group(0));
            System.out.println(matcher.group(1));
            System.out.println(matcher.group(2));
        }
    }
}
```

其中：

1. `group(0)` 表示整个匹配内容。
2. `group(1)` 表示第一个分组。
3. `group(2)` 表示第二个分组。

## 5. 日期时间 API

### 5.1 为什么需要新的日期时间 API

Java 早期有 `Date` 和 `Calendar`，但它们使用起来不够直观。现代 Java 通常优先使用 `java.time` 包中的日期时间 API。

常用类型：

| 类型 | 含义 |
| ---- | ---- |
| `LocalDate` | 日期，例如 2026-06-22 |
| `LocalTime` | 时间，例如 10:30:15 |
| `LocalDateTime` | 日期和时间 |
| `Instant` | 时间戳 |
| `Duration` | 时间间隔，通常用于时分秒 |
| `Period` | 日期间隔，通常用于年月日 |
| `DateTimeFormatter` | 日期时间格式化工具 |

### 5.2 `LocalDate`

```java
import java.time.LocalDate;

public class LocalDateDemo {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();
        LocalDate birthday = LocalDate.of(2000, 5, 20);

        System.out.println(today);
        System.out.println(birthday);
        System.out.println(today.getYear());
        System.out.println(today.getMonthValue());
        System.out.println(today.getDayOfMonth());
    }
}
```

`LocalDate` 只表示日期，不包含具体时间。

### 5.3 `LocalTime` 和 `LocalDateTime`

```java
import java.time.LocalDateTime;
import java.time.LocalTime;

public class LocalTimeDemo {
    public static void main(String[] args) {
        LocalTime time = LocalTime.now();
        LocalDateTime dateTime = LocalDateTime.now();

        System.out.println(time);
        System.out.println(dateTime);
    }
}
```

`LocalTime` 表示时间，`LocalDateTime` 表示日期和时间。

### 5.4 日期时间加减

```java
import java.time.LocalDate;
import java.time.LocalDateTime;

public class DateTimeCalculateDemo {
    public static void main(String[] args) {
        LocalDate today = LocalDate.now();

        System.out.println(today.plusDays(7));
        System.out.println(today.minusMonths(1));

        LocalDateTime now = LocalDateTime.now();
        System.out.println(now.plusHours(2));
    }
}
```

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `plusDays` | 增加天数 |
| `minusDays` | 减少天数 |
| `plusMonths` | 增加月数 |
| `minusMonths` | 减少月数 |
| `plusYears` | 增加年数 |
| `minusYears` | 减少年数 |

### 5.5 格式化和解析

`DateTimeFormatter` 用于日期时间格式化和解析。

```java
import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;

public class DateTimeFormatterDemo {
    public static void main(String[] args) {
        LocalDateTime now = LocalDateTime.now();

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        String text = now.format(formatter);

        System.out.println(text);

        LocalDateTime parsed = LocalDateTime.parse("2026-06-22 10:30:00", formatter);
        System.out.println(parsed);
    }
}
```

常见格式符号：

| 符号 | 含义 |
| ---- | ---- |
| `yyyy` | 年 |
| `MM` | 月 |
| `dd` | 日 |
| `HH` | 24 小时制小时 |
| `mm` | 分钟 |
| `ss` | 秒 |

注意：`MM` 表示月份，`mm` 表示分钟，大小写不能混用。

### 5.6 `Duration` 和 `Period`

`Duration` 通常用于计算时间间隔。

```java
import java.time.Duration;
import java.time.LocalDateTime;

public class DurationDemo {
    public static void main(String[] args) {
        LocalDateTime start = LocalDateTime.of(2026, 6, 22, 10, 0);
        LocalDateTime end = LocalDateTime.of(2026, 6, 22, 12, 30);

        Duration duration = Duration.between(start, end);

        System.out.println(duration.toHours());
        System.out.println(duration.toMinutes());
    }
}
```

`Period` 通常用于计算日期间隔。

```java
import java.time.LocalDate;
import java.time.Period;

public class PeriodDemo {
    public static void main(String[] args) {
        LocalDate start = LocalDate.of(2020, 1, 1);
        LocalDate end = LocalDate.of(2026, 6, 22);

        Period period = Period.between(start, end);

        System.out.println(period.getYears());
        System.out.println(period.getMonths());
        System.out.println(period.getDays());
    }
}
```

## 6. Lambda 表达式

### 6.1 为什么需要 Lambda

在学习 `Comparator` 时，排序规则可以使用匿名内部类编写。

```java
import java.util.ArrayList;
import java.util.Comparator;

public class AnonymousComparatorDemo {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();
        numbers.add(30);
        numbers.add(10);
        numbers.add(20);

        numbers.sort(new Comparator<Integer>() {
            @Override
            public int compare(Integer a, Integer b) {
                return a - b;
            }
        });

        System.out.println(numbers);
    }
}
```

这段代码真正重要的是 `return a - b;`，但为了表达排序规则，写了很多结构代码。

Lambda 可以简化这种写法。

```java
numbers.sort((a, b) -> a - b);
```

### 6.2 Lambda 基本语法

Lambda 基本格式：

```text
(参数列表) -> {
    方法体;
}
```

示例：

```java
import java.util.ArrayList;

public class LambdaDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        names.forEach(name -> System.out.println(name));
    }
}
```

`name -> System.out.println(name)` 表示：接收一个元素 `name`，然后输出它。

### 6.3 Lambda 简化规则

常见简化规则：

1. 参数类型可以省略。
2. 只有一个参数时，小括号可以省略。
3. 方法体只有一条语句时，大括号可以省略。
4. 方法体只有一个返回表达式时，`return` 可以省略。

示例：

```java
Comparator<Integer> c1 = (Integer a, Integer b) -> {
    return a - b;
};

Comparator<Integer> c2 = (a, b) -> {
    return a - b;
};

Comparator<Integer> c3 = (a, b) -> a - b;
```

### 6.4 函数式接口

Lambda 只能用于函数式接口。

函数式接口是只有一个抽象方法的接口。

```java
@FunctionalInterface
public interface Calculator {
    int calculate(int a, int b);
}
```

使用 Lambda：

```java
public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        Calculator add = (a, b) -> a + b;
        Calculator multiply = (a, b) -> a * b;

        System.out.println(add.calculate(10, 20));
        System.out.println(multiply.calculate(10, 20));
    }
}
```

`@FunctionalInterface` 不是必须的，但它可以让编译器检查该接口是否符合函数式接口规则。

### 6.5 常用函数式接口

Java 在 `java.util.function` 包中提供了一些常用函数式接口。

| 接口 | 抽象方法 | 含义 |
| ---- | -------- | ---- |
| `Consumer<T>` | `accept(T t)` | 接收一个值，不返回结果 |
| `Supplier<T>` | `get()` | 不接收参数，返回一个值 |
| `Function<T, R>` | `apply(T t)` | 接收一个值，返回一个值 |
| `Predicate<T>` | `test(T t)` | 接收一个值，返回布尔结果 |

示例：

```java
import java.util.function.Consumer;
import java.util.function.Function;
import java.util.function.Predicate;
import java.util.function.Supplier;

public class FunctionInterfaceDemo {
    public static void main(String[] args) {
        Consumer<String> printer = text -> System.out.println(text);
        Supplier<Integer> numberSupplier = () -> 100;
        Function<String, Integer> lengthFunction = text -> text.length();
        Predicate<Integer> passPredicate = score -> score >= 60;

        printer.accept("Java");
        System.out.println(numberSupplier.get());
        System.out.println(lengthFunction.apply("Java"));
        System.out.println(passPredicate.test(80));
    }
}
```

### 6.6 方法引用

方法引用是 Lambda 的进一步简化。

```java
names.forEach(name -> System.out.println(name));
```

可以简化为：

```java
names.forEach(System.out::println);
```

常见方法引用形式：

| 形式 | 示例 |
| ---- | ---- |
| 对象名::实例方法 | `System.out::println` |
| 类名::静态方法 | `Integer::parseInt` |
| 类名::实例方法 | `String::length` |
| 类名::new | `ArrayList::new` |

示例：

```java
import java.util.ArrayList;

public class MethodReferenceDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");

        names.forEach(System.out::println);
    }
}
```

## 7. Stream API

### 7.1 什么是 Stream

Stream 是 Java 提供的数据处理 API。

它可以对集合中的数据进行链式处理，例如：

1. 筛选。
2. 转换。
3. 排序。
4. 去重。
5. 截取。
6. 统计。
7. 收集结果。

Stream 不等于集合。集合负责保存数据，Stream 负责处理数据。

### 7.2 Stream 基本流程

Stream 通常包含三个步骤：

1. 创建 Stream。
2. 中间操作。
3. 终结操作。

示例：

```java
import java.util.ArrayList;
import java.util.List;

public class StreamIntroDemo {
    public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();
        scores.add(95);
        scores.add(88);
        scores.add(58);
        scores.add(76);

        List<Integer> passed = scores.stream()
                .filter(score -> score >= 60)
                .toList();

        System.out.println(passed);
    }
}
```

其中：

1. `scores.stream()` 创建 Stream。
2. `filter(score -> score >= 60)` 筛选及格成绩。
3. `toList()` 收集结果。

### 7.3 常见中间操作

中间操作返回新的 Stream，可以继续链式调用。

| 方法 | 含义 |
| ---- | ---- |
| `filter` | 筛选 |
| `map` | 转换 |
| `sorted` | 排序 |
| `distinct` | 去重 |
| `limit` | 截取前几个 |
| `skip` | 跳过前几个 |

示例：

```java
import java.util.ArrayList;
import java.util.List;

public class StreamMiddleDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");
        names.add("Alice");

        List<String> result = names.stream()
                .distinct()
                .filter(name -> name.length() > 3)
                .sorted()
                .toList();

        System.out.println(result);
    }
}
```

### 7.4 `map` 转换

`map` 用于把一种数据转换成另一种数据。

```java
import java.util.ArrayList;
import java.util.List;

public class StreamMapDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");
        names.add("Cindy");

        List<Integer> lengths = names.stream()
                .map(name -> name.length())
                .toList();

        System.out.println(lengths);
    }
}
```

### 7.5 常见终结操作

终结操作会结束 Stream。

| 方法 | 含义 |
| ---- | ---- |
| `forEach` | 遍历 |
| `count` | 统计数量 |
| `toList` | 收集为列表 |
| `anyMatch` | 是否任意元素匹配 |
| `allMatch` | 是否所有元素匹配 |
| `noneMatch` | 是否没有元素匹配 |
| `findFirst` | 查找第一个元素 |
| `reduce` | 归约计算 |

示例：

```java
import java.util.ArrayList;

public class StreamTerminalDemo {
    public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();
        scores.add(95);
        scores.add(88);
        scores.add(58);

        long passCount = scores.stream()
                .filter(score -> score >= 60)
                .count();

        boolean hasFullScore = scores.stream()
                .anyMatch(score -> score == 100);

        System.out.println(passCount);
        System.out.println(hasFullScore);
    }
}
```

### 7.6 `reduce`

`reduce` 可以把多个数据合并成一个结果。

```java
import java.util.ArrayList;

public class StreamReduceDemo {
    public static void main(String[] args) {
        ArrayList<Integer> numbers = new ArrayList<>();
        numbers.add(10);
        numbers.add(20);
        numbers.add(30);

        int total = numbers.stream()
                .reduce(0, (a, b) -> a + b);

        System.out.println(total);
    }
}
```

其中：

1. `0` 是初始值。
2. `(a, b) -> a + b` 表示合并规则。

### 7.7 Stream 注意事项

使用 Stream 时应注意：

1. Stream 不保存数据。
2. Stream 通常不直接修改原集合。
3. 中间操作是延迟执行的。
4. 终结操作执行后，Stream 不能再次使用。
5. 复杂逻辑不要为了链式写法牺牲可读性。

示例：

```java
import java.util.ArrayList;
import java.util.stream.Stream;

public class StreamReuseDemo {
    public static void main(String[] args) {
        ArrayList<String> names = new ArrayList<>();
        names.add("Alice");
        names.add("Bob");

        Stream<String> stream = names.stream();
        stream.forEach(System.out::println);

        // stream.count(); // 终结操作后不能再次使用同一个 Stream
    }
}
```

## 8. 多线程基础

### 8.1 程序、进程和线程

程序是写好的代码和文件，例如一个 `.class` 文件或一个应用程序。

进程是正在运行的程序。

线程是进程中的执行路径。一个进程中可以有多个线程。

示例：

```text
一个音乐播放器进程中可能同时有多个线程：
1. 播放音乐。
2. 更新界面。
3. 下载歌词。
```

多线程可以让程序同时执行多个任务。

### 8.2 创建线程：继承 `Thread`

```java
public class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(getName() + " - " + i);
        }
    }
}
```

使用：

```java
public class ThreadDemo {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.setName("线程 A");
        t2.setName("线程 B");

        t1.start();
        t2.start();
    }
}
```

注意：启动线程应调用 `start()`，不是直接调用 `run()`。

### 8.3 创建线程：实现 `Runnable`

```java
public class MyTask implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println(Thread.currentThread().getName() + " - " + i);
        }
    }
}
```

使用：

```java
public class RunnableDemo {
    public static void main(String[] args) {
        MyTask task = new MyTask();

        Thread t1 = new Thread(task, "线程 A");
        Thread t2 = new Thread(task, "线程 B");

        t1.start();
        t2.start();
    }
}
```

`Runnable` 更适合把任务和线程对象分开。

### 8.4 使用 Lambda 创建线程任务

`Runnable` 是函数式接口，因此可以使用 Lambda。

```java
public class RunnableLambdaDemo {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println(Thread.currentThread().getName() + " - " + i);
            }
        }, "Lambda线程");

        thread.start();
    }
}
```

### 8.5 `sleep` 和 `join`

`sleep` 让当前线程暂停一段时间。

```java
public class SleepDemo {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            System.out.println(i);

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}
```

`join` 表示等待另一个线程执行结束。

```java
public class JoinDemo {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 5; i++) {
                System.out.println("子线程：" + i);
            }
        });

        thread.start();

        try {
            thread.join();
        } catch (InterruptedException e) {
            Thread.currentThread().interrupt();
        }

        System.out.println("主线程结束");
    }
}
```

### 8.6 线程安全问题

多个线程同时操作共享数据时，可能出现线程安全问题。

示例：

```java
public class Counter {
    private int count;

    public void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

`count++` 看起来是一行代码，但它不是一个不可拆分的操作。多个线程同时执行时，可能导致结果不正确。

### 8.7 `synchronized`

`synchronized` 可以让同一时刻只有一个线程进入某段关键代码。

```java
public class SafeCounter {
    private int count;

    public synchronized void increment() {
        count++;
    }

    public int getCount() {
        return count;
    }
}
```

也可以同步代码块：

```java
public class SafeCounterBlock {
    private int count;
    private final Object lock = new Object();

    public void increment() {
        synchronized (lock) {
            count++;
        }
    }

    public int getCount() {
        return count;
    }
}
```

### 8.8 `volatile` 基础

`volatile` 用于保证变量在线程之间的可见性。

```java
public class FlagTask implements Runnable {
    private volatile boolean running = true;

    @Override
    public void run() {
        while (running) {
            // 执行任务
        }
    }

    public void stop() {
        running = false;
    }
}
```

基础阶段只需要记住：

1. `volatile` 能保证可见性。
2. `volatile` 不能保证复合操作的原子性。
3. `count++` 这类操作不能只靠 `volatile` 保证线程安全。

## 9. 线程池

### 9.1 为什么需要线程池

频繁创建和销毁线程会消耗资源。

线程池可以复用线程，统一管理任务。

常见优点：

1. 减少线程创建成本。
2. 控制线程数量。
3. 统一管理任务执行。
4. 更适合真实项目。

### 9.2 `ExecutorService`

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorServiceDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newFixedThreadPool(3);

        for (int i = 0; i < 5; i++) {
            int taskId = i;

            executor.submit(() -> {
                System.out.println(Thread.currentThread().getName() + " 执行任务 " + taskId);
            });
        }

        executor.shutdown();
    }
}
```

其中：

1. `newFixedThreadPool(3)` 创建固定大小线程池。
2. `submit` 提交任务。
3. `shutdown` 表示不再接收新任务，并在已提交任务完成后关闭。

### 9.3 `Callable` 和 `Future`

`Runnable` 没有返回值。`Callable` 可以返回结果，也可以抛出异常。

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

public class CallableFutureDemo {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();

        Callable<Integer> task = () -> 100 + 200;

        Future<Integer> future = executor.submit(task);

        try {
            Integer result = future.get();
            System.out.println(result);
        } catch (Exception e) {
            System.out.println("任务执行失败：" + e.getMessage());
        }

        executor.shutdown();
    }
}
```

其中：

1. `Callable<Integer>` 表示任务返回 `Integer`。
2. `future.get()` 获取任务执行结果。
3. `get()` 会等待任务完成。

### 9.4 `CompletableFuture` 入门

`CompletableFuture` 可以更方便地编写异步任务。

```java
import java.util.concurrent.CompletableFuture;

public class CompletableFutureDemo {
    public static void main(String[] args) {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            return "Hello";
        });

        future.thenApply(text -> text + " Java")
                .thenAccept(System.out::println)
                .join();
    }
}
```

其中：

1. `supplyAsync` 异步执行有返回值的任务。
2. `thenApply` 对结果进行转换。
3. `thenAccept` 消费结果。
4. `join` 等待异步任务完成。

基础阶段只要求能看懂和简单使用，不要求深入掌握复杂异步编排。

## 10. 代码示例

在 IDEA 中创建以下文件，并保证它们位于同一个包或同一个源码目录下。

### 10.1 `TaskStatus.java`

```java
public enum TaskStatus {
    TODO("待处理"),
    DOING("处理中"),
    DONE("已完成");

    private String description;

    TaskStatus(String description) {
        this.description = description;
    }

    public String getDescription() {
        return description;
    }
}
```

### 10.2 `LearningTask.java`

```java
import java.time.LocalDate;

public class LearningTask {
    private String title;
    private TaskStatus status;
    private LocalDate deadline;
    private int score;

    public LearningTask(String title, TaskStatus status, LocalDate deadline, int score) {
        this.title = title;
        this.status = status;
        this.deadline = deadline;
        this.score = score;
    }

    public String getTitle() {
        return title;
    }

    public TaskStatus getStatus() {
        return status;
    }

    public LocalDate getDeadline() {
        return deadline;
    }

    public int getScore() {
        return score;
    }

    @Override
    public String toString() {
        return title + "-" + status.getDescription() + "-" + deadline + "-" + score;
    }
}
```

### 10.3 `Task6Demo.java`

```java
import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.CompletableFuture;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class Task6Demo {
    public static void main(String[] args) {
        ArrayList<LearningTask> tasks = new ArrayList<>();
        tasks.add(new LearningTask("Lambda", TaskStatus.DONE, LocalDate.of(2026, 6, 20), 95));
        tasks.add(new LearningTask("Stream", TaskStatus.DOING, LocalDate.of(2026, 6, 22), 88));
        tasks.add(new LearningTask("Thread", TaskStatus.TODO, LocalDate.of(2026, 6, 25), 76));
        tasks.add(new LearningTask("Regex", TaskStatus.DONE, LocalDate.of(2026, 6, 18), 90));

        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");

        tasks.forEach(task -> {
            String deadline = task.getDeadline().format(formatter);
            System.out.println(task.getTitle() + " 截止时间：" + deadline);
        });

        List<String> doneTaskNames = tasks.stream()
                .filter(task -> task.getStatus() == TaskStatus.DONE)
                .sorted((a, b) -> b.getScore() - a.getScore())
                .map(LearningTask::getTitle)
                .toList();

        System.out.println(doneTaskNames);

        String text = "Alice:95, Bob:88, Cindy:76";
        Pattern pattern = Pattern.compile("([A-Za-z]+):(\\d+)");
        Matcher matcher = pattern.matcher(text);

        while (matcher.find()) {
            System.out.println(matcher.group(1) + " -> " + matcher.group(2));
        }

        CompletableFuture<Integer> future = CompletableFuture.supplyAsync(() -> {
            return tasks.stream()
                    .map(LearningTask::getScore)
                    .reduce(0, Integer::sum);
        });

        future.thenAccept(total -> System.out.println("总分：" + total)).join();
    }
}
```

运行后应重点观察以下内容：

1. `TaskStatus` 如何使用枚举表示固定状态。
2. `LocalDate` 如何表示任务截止日期。
3. `DateTimeFormatter` 如何格式化日期。
4. `forEach` 如何配合 Lambda 遍历集合。
5. `stream` 如何筛选、排序和转换数据。
6. `Pattern` 和 `Matcher` 如何提取文本。
7. `CompletableFuture` 如何异步计算结果。

## 11. 练习

### 11.1 枚举练习

创建 `EnumPractice.java`，完成以下要求：

1. 定义 `PaymentStatus` 枚举。
2. 枚举常量包括 `UNPAID`、`PAID`、`REFUNDED`、`CLOSED`。
3. 为每个枚举常量设置中文描述。
4. 定义 `getDescription` 方法。
5. 使用 `values` 遍历所有状态。
6. 使用 `valueOf` 根据字符串获取枚举。
7. 使用 `switch` 根据状态输出不同提示。

### 11.2 正则练习

创建 `RegexPractice.java`，完成以下要求：

1. 判断手机号是否符合 `1[3-9]` 开头且总长度为 11 位。
2. 判断字符串是否为纯数字。
3. 使用 `replaceAll` 将字符串中的数字替换为 `#`。
4. 使用 `split` 按多个空格拆分字符串。
5. 使用 `Pattern` 和 `Matcher` 从 `"Alice:95, Bob:88, Cindy:76"` 中提取姓名和成绩。
6. 使用分组分别输出姓名和成绩。

### 11.3 日期时间练习

创建 `DateTimePractice.java`，完成以下要求：

1. 使用 `LocalDate.now` 获取当前日期。
2. 使用 `LocalDate.of` 创建生日日期。
3. 输出当前日期的年、月、日。
4. 使用 `LocalDateTime.now` 获取当前日期时间。
5. 使用 `DateTimeFormatter` 格式化为 `yyyy-MM-dd HH:mm:ss`。
6. 将字符串 `2026-06-22 10:30:00` 解析为 `LocalDateTime`。
7. 使用 `Period` 计算两个日期相差的年月日。
8. 使用 `Duration` 计算两个时间相差的分钟数。

### 11.4 Lambda 与函数式接口练习

创建 `LambdaPractice.java`，完成以下要求：

1. 定义函数式接口 `StringChecker`，包含 `boolean check(String value)` 方法。
2. 使用 Lambda 判断字符串长度是否大于 5。
3. 使用 `Consumer<String>` 输出字符串。
4. 使用 `Supplier<Integer>` 返回随机整数。
5. 使用 `Function<String, Integer>` 返回字符串长度。
6. 使用 `Predicate<Integer>` 判断成绩是否及格。
7. 使用方法引用 `System.out::println` 输出集合元素。

### 11.5 Stream 练习

创建 `StreamPractice.java`，完成以下要求：

1. 定义 `Student` 类，包含 `name`、`age`、`score`。
2. 创建 `ArrayList<Student>` 保存至少 5 个学生。
3. 使用 `filter` 筛选成绩大于等于 60 的学生。
4. 使用 `map` 获取所有学生姓名。
5. 使用 `sorted` 按成绩从高到低排序。
6. 使用 `count` 统计及格人数。
7. 使用 `anyMatch` 判断是否存在满分学生。
8. 使用 `reduce` 计算总分。
9. 使用 `toList` 收集筛选后的学生。

### 11.6 多线程练习

创建 `ThreadPractice.java`，完成以下要求：

1. 使用继承 `Thread` 的方式创建线程。
2. 使用实现 `Runnable` 的方式创建线程。
3. 使用 Lambda 创建线程任务。
4. 使用 `Thread.sleep` 暂停线程。
5. 使用 `join` 等待子线程结束。
6. 创建共享计数器，演示多线程修改共享数据。
7. 使用 `synchronized` 保证计数器线程安全。
8. 使用 `volatile` 控制线程停止标记。

### 11.7 线程池练习

创建 `ExecutorPractice.java`，完成以下要求：

1. 使用 `Executors.newFixedThreadPool` 创建线程池。
2. 使用 `submit` 提交 5 个任务。
3. 输出每个任务对应的线程名称。
4. 使用 `Callable<Integer>` 创建有返回值任务。
5. 使用 `Future.get` 获取任务结果。
6. 使用 `shutdown` 关闭线程池。
7. 使用 `CompletableFuture.supplyAsync` 创建异步任务。
8. 使用 `thenApply` 转换结果。
9. 使用 `thenAccept` 输出结果。

### 11.8 综合练习

创建 `Task6Practice.java`，完成以下要求：

1. 定义 `CourseStatus` 枚举，包含 `NOT_STARTED`、`LEARNING`、`FINISHED`。
2. 定义 `Course` 类，包含 `name`、`status`、`startDate`、`score`。
3. `startDate` 使用 `LocalDate`。
4. 创建至少 6 个课程对象。
5. 使用正则校验课程名称只能由字母、数字和中文组成。
6. 使用 `DateTimeFormatter` 格式化课程开始日期。
7. 使用 Lambda 遍历输出课程信息。
8. 使用 Stream 筛选已完成课程。
9. 使用 Stream 按成绩从高到低排序。
10. 使用 Stream 统计平均分。
11. 使用线程池异步计算所有课程总分。
12. 使用 `CompletableFuture` 输出异步计算结果。
13. 使用注释说明程序中哪里使用了枚举、正则、日期时间、Lambda、Stream 和多线程。

## 12. 验收标准

满足以下条件视为完成 Task 6：

1. 能说明枚举的作用。
2. 能定义枚举常量、成员变量、构造方法和成员方法。
3. 能使用 `values`、`valueOf`、`name`。
4. 能使用枚举配合 `switch`。
5. 能说明正则表达式的作用。
6. 能写出基础正则表达式。
7. 能使用 `matches`、`replaceAll`、`split`。
8. 能使用 `Pattern` 和 `Matcher` 查找文本。
9. 能使用正则分组提取内容。
10. 能使用 `LocalDate`、`LocalTime`、`LocalDateTime`。
11. 能使用 `DateTimeFormatter` 格式化和解析日期时间。
12. 能使用 `Duration` 和 `Period` 计算时间差。
13. 能说明 Lambda 表达式的作用。
14. 能定义和使用函数式接口。
15. 能使用常见函数式接口。
16. 能使用方法引用。
17. 能使用 Stream 完成筛选、转换、排序、统计和收集。
18. 能说明程序、进程和线程的区别。
19. 能使用 `Thread`、`Runnable` 和 Lambda 创建线程任务。
20. 能使用 `sleep` 和 `join`。
21. 能说明线程安全问题。
22. 能使用 `synchronized` 保护共享数据。
23. 能说明 `volatile` 的基础作用。
24. 能使用线程池提交任务。
25. 能使用 `Callable`、`Future` 和 `CompletableFuture`。
26. 能独立完成 `Task6Practice.java` 并运行得到正确结果。

## 13. 提交规范

1. 学习/实验总结报告统一命名为 `Task_6_report.md`，放置在 `Task_6/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_6/src/` 目录下（或在 `Task_6/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_6/` 目录中的本阶段内容。
