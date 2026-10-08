# Task 5：Java 异常处理与 IO

## 1. 任务说明

本任务引入 Java 异常处理机制和 IO 基础，重点包括异常体系、异常捕获、异常抛出、自定义异常、标准输入输出、文件对象、字节流、字符流、缓冲流、转换流、打印流、对象序列化以及 `Files` 工具类的基础使用。

在前面的任务中，程序通常只处理“正常情况”。但实际程序中经常会出现各种异常情况，例如数组索引越界、字符串转数字失败、文件不存在、读取文件失败、写入文件失败等。异常处理用于让程序面对错误情况时可以给出明确处理方式，而不是直接中断或得到难以理解的报错。

IO 是 Input 和 Output 的缩写，表示输入和输出。输入是程序从外部读取数据，输出是程序向外部写出数据。键盘输入、控制台打印、文件读取、文件写入，都属于 IO 的典型场景。

本任务也会解释前面学习时暂时“先照着写”的内容：

1. 为什么 `Scanner` 创建对象时经常传入 `System.in`。
2. 为什么打印时使用 `System.out.println`。
3. `System.in`、`System.out`、`System.err` 分别是什么。
4. 为什么 Java 25 及之后可以使用 `IO.println` 这种更简洁的打印方式。

## 2. 知识范围

本任务包含以下内容：

1. 异常的含义。
2. 异常体系。
3. `Throwable`、`Error`、`Exception`。
4. 运行时异常和受检异常。
5. 常见运行时异常。
6. `try-catch`。
7. 多个 `catch`。
8. `finally`。
9. `throw` 和 `throws`。
10. 自定义异常。
11. 异常处理原则。
12. IO 的含义。
13. `System.in`、`System.out`、`System.err`。
14. `Scanner` 与标准输入。
15. Java 25 及之后的 `IO.println`。
16. `File`。
17. 字节流。
18. 字符流。
19. 缓冲流。
20. 转换流与字符编码。
21. 打印流。
22. 对象序列化。
23. `Path` 与 `Files`。
24. `try-with-resources`。

## 3. 异常基础

### 3.1 什么是异常

异常是程序运行过程中出现的非正常情况。

示例：

```java
public class ExceptionIntroDemo {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30};

        System.out.println(numbers[5]);
    }
}
```

数组长度为 `3`，有效索引是 `0`、`1`、`2`。访问 `numbers[5]` 时，程序会出现数组索引越界异常。

常见异常情况包括：

1. 数组索引越界。
2. 空引用调用方法。
3. 字符串转换数字失败。
4. 除数为 `0`。
5. 文件不存在。
6. 文件读取失败。
7. 文件写入失败。

异常并不是简单的“语法错误”。语法错误会导致程序无法编译；异常通常是程序运行时出现的问题。

### 3.2 为什么需要异常处理

如果程序没有处理异常，一旦运行过程中出现异常，程序通常会中断，并在控制台输出异常信息。

```java
public class NoHandleDemo {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;

        int result = a / b;

        System.out.println(result);
        System.out.println("程序结束");
    }
}
```

运行时，`a / b` 会出现除数为 `0` 的异常，后面的输出语句不会继续正常执行。

异常处理的作用包括：

1. 捕获异常并给出友好提示。
2. 避免程序因为局部错误直接中断。
3. 将错误信息传递给调用者。
4. 保证资源在使用后可以被关闭。

### 3.3 异常对象

在 Java 中，异常也是对象。

当异常发生时，Java 会创建一个异常对象，这个对象中包含异常类型、异常信息和异常发生的位置。

示例输出可能类似：

```text
Exception in thread "main" java.lang.ArithmeticException: / by zero
    at DivideDemo.main(DivideDemo.java:6)
```

其中：

1. `ArithmeticException` 是异常类型。
2. `/ by zero` 是异常信息。
3. `DivideDemo.java:6` 表示异常发生在该文件第 6 行。

读懂异常信息是调试 Java 程序的重要能力。

## 4. 异常体系

### 4.1 `Throwable`

Java 中所有可以被抛出的错误或异常，最终都继承自 `Throwable`。

异常体系可以简单理解为：

```text
Throwable
├── Error
└── Exception
    ├── RuntimeException
    └── 其他受检异常
```

其中：

1. `Throwable` 是顶层父类。
2. `Error` 表示严重错误。
3. `Exception` 表示程序可以处理的异常。
4. `RuntimeException` 表示运行时异常。

### 4.2 `Error`

`Error` 表示 Java 虚拟机或运行环境中的严重问题。

常见示例：

1. 内存溢出。
2. 栈溢出。
3. 虚拟机内部错误。

基础阶段不要求主动处理 `Error`。通常程序员重点处理的是 `Exception`。

### 4.3 `Exception`

`Exception` 表示程序可以处理的异常。

例如：

1. `IOException`：输入输出异常。
2. `FileNotFoundException`：文件找不到。
3. `ClassNotFoundException`：类找不到。
4. `RuntimeException` 及其子类：运行时异常。

`Exception` 又可以分为两类：

1. 运行时异常。
2. 受检异常。

### 4.4 运行时异常

运行时异常是 `RuntimeException` 及其子类。

常见运行时异常：

| 异常类型 | 常见原因 |
| -------- | -------- |
| `NullPointerException` | 空引用调用成员 |
| `ArrayIndexOutOfBoundsException` | 数组索引越界 |
| `StringIndexOutOfBoundsException` | 字符串索引越界 |
| `ArithmeticException` | 数学运算错误，例如除数为 `0` |
| `NumberFormatException` | 字符串转换数字失败 |
| `ClassCastException` | 类型转换失败 |
| `IllegalArgumentException` | 方法参数不合法 |

运行时异常通常由代码逻辑问题引起。编译器不会强制要求处理它们。

```java
public class RuntimeExceptionDemo {
    public static void main(String[] args) {
        String text = "abc";
        int number = Integer.parseInt(text);

        System.out.println(number);
    }
}
```

上述代码可以通过编译，但运行时会出现 `NumberFormatException`。

### 4.5 受检异常

受检异常是编译器会检查的异常。

如果某段代码可能抛出受检异常，编译器会要求程序员必须处理它，或者在方法声明中继续抛出它。

常见受检异常：

| 异常类型 | 常见场景 |
| -------- | -------- |
| `IOException` | 输入输出失败 |
| `FileNotFoundException` | 文件不存在 |
| `ClassNotFoundException` | 类加载失败 |
| `InterruptedException` | 线程等待被中断 |

示例：

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class CheckedExceptionDemo {
    public static void main(String[] args) throws FileNotFoundException {
        FileInputStream input = new FileInputStream("data.txt");
    }
}
```

`FileInputStream` 创建对象时可能找不到文件，因此会涉及 `FileNotFoundException`。

### 4.6 运行时异常与受检异常的区别

| 对比项 | 运行时异常 | 受检异常 |
| ------ | ---------- | -------- |
| 父类 | `RuntimeException` | `Exception` 但不是 `RuntimeException` |
| 编译器是否强制处理 | 不强制 | 强制 |
| 常见原因 | 代码逻辑问题 | 外部环境或资源问题 |
| 常见场景 | 空引用、越界、格式错误 | 文件、网络、线程等 |

基础阶段可以这样理解：

1. 运行时异常通常应通过修改代码逻辑来避免。
2. 受检异常通常需要通过 `try-catch` 或 `throws` 明确处理。

## 5. 常见运行时异常

### 5.1 空引用异常

当引用变量为 `null`，却调用它的成员变量或方法时，会出现空引用异常。

```java
public class NullPointerDemo {
    public static void main(String[] args) {
        String text = null;

        System.out.println(text.length());
    }
}
```

`text` 没有指向任何对象，因此不能调用 `length()`。

常见避免方式：

```java
public class NullCheckDemo {
    public static void main(String[] args) {
        String text = null;

        if (text != null) {
            System.out.println(text.length());
        } else {
            System.out.println("text 没有指向对象");
        }
    }
}
```

### 5.2 数组索引越界异常

```java
public class ArrayIndexDemo {
    public static void main(String[] args) {
        int[] scores = {95, 88, 76};

        for (int i = 0; i <= scores.length; i++) {
            System.out.println(scores[i]);
        }
    }
}
```

错误原因是循环条件写成了 `i <= scores.length`。

正确写法：

```java
for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```

### 5.3 数字格式异常

```java
public class NumberFormatDemo {
    public static void main(String[] args) {
        String text = "12a";
        int number = Integer.parseInt(text);

        System.out.println(number);
    }
}
```

`"12a"` 不是合法整数格式，因此会出现 `NumberFormatException`。

可以先判断字符串内容是否符合要求：

```java
public class NumberCheckDemo {
    public static void main(String[] args) {
        String text = "123";

        if (text.matches("\\d+")) {
            int number = Integer.parseInt(text);
            System.out.println(number);
        } else {
            System.out.println("不是合法整数");
        }
    }
}
```

### 5.4 类型转换异常

```java
public class CastExceptionDemo {
    public static void main(String[] args) {
        Object value = "Java";
        Integer number = (Integer) value;

        System.out.println(number);
    }
}
```

`value` 实际指向的是 `String` 对象，不能强制转换为 `Integer`。

可以使用 `instanceof` 判断：

```java
public class CastCheckDemo {
    public static void main(String[] args) {
        Object value = "Java";

        if (value instanceof Integer) {
            Integer number = (Integer) value;
            System.out.println(number);
        } else {
            System.out.println("value 不是 Integer");
        }
    }
}
```

## 6. `try-catch`

### 6.1 基本语法

`try-catch` 用于捕获并处理异常。

```java
try {
    可能出现异常的代码;
} catch (异常类型 变量名) {
    异常处理代码;
}
```

示例：

```java
public class TryCatchDemo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("除数不能为 0");
        }

        System.out.println("程序继续执行");
    }
}
```

其中：

1. `try` 中写可能出现异常的代码。
2. `catch` 中写捕获到异常后的处理方式。
3. `ArithmeticException e` 表示捕获一个算术异常对象。
4. 异常被处理后，程序可以继续执行后面的代码。

### 6.2 异常对象常用方法

异常对象常用方法：

| 方法 | 含义 |
| ---- | ---- |
| `getMessage()` | 获取异常信息 |
| `toString()` | 获取异常类型和异常信息 |
| `printStackTrace()` | 打印异常堆栈信息 |

示例：

```java
public class ExceptionMethodDemo {
    public static void main(String[] args) {
        try {
            int number = Integer.parseInt("abc");
            System.out.println(number);
        } catch (NumberFormatException e) {
            System.out.println(e.getMessage());
            System.out.println(e.toString());
            e.printStackTrace();
        }
    }
}
```

学习阶段可以使用 `printStackTrace()` 查看完整错误位置。实际输出可能比较长，但它能帮助定位异常发生在哪个文件、哪个方法、哪一行。

### 6.3 捕获多个异常

一段代码中可能出现多种异常，可以写多个 `catch`。

```java
public class MultiCatchDemo {
    public static void main(String[] args) {
        String[] values = {"10", "0"};

        try {
            int a = Integer.parseInt(values[0]);
            int b = Integer.parseInt(values[1]);
            int result = a / b;

            System.out.println(result);
        } catch (NumberFormatException e) {
            System.out.println("数字格式不正确");
        } catch (ArithmeticException e) {
            System.out.println("除数不能为 0");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组索引越界");
        }
    }
}
```

多个 `catch` 会从上到下依次匹配，匹配成功后执行对应处理代码。

### 6.4 父类异常和子类异常顺序

如果多个 `catch` 中既有父类异常，也有子类异常，应先写子类，再写父类。

正确写法：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (NumberFormatException e) {
    System.out.println("数字格式不正确");
} catch (Exception e) {
    System.out.println("其他异常");
}
```

错误写法：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (Exception e) {
    System.out.println("其他异常");
} catch (NumberFormatException e) {
    System.out.println("数字格式不正确");
}
```

如果先捕获 `Exception`，后面的 `NumberFormatException` 永远没有机会执行，因为 `NumberFormatException` 本身也是一种 `Exception`。

### 6.5 多异常合并捕获

如果多个异常的处理方式完全相同，可以使用 `|` 合并捕获。

```java
public class UnionCatchDemo {
    public static void main(String[] args) {
        try {
            String text = "abc";
            int number = Integer.parseInt(text);
            int result = 100 / number;

            System.out.println(result);
        } catch (NumberFormatException | ArithmeticException e) {
            System.out.println("数据无法用于计算");
        }
    }
}
```

合并捕获适合处理逻辑完全相同的情况。

## 7. `finally`

### 7.1 `finally` 的含义

`finally` 中的代码通常用于释放资源，无论是否出现异常，通常都会执行。

```java
try {
    可能出现异常的代码;
} catch (异常类型 变量名) {
    异常处理代码;
} finally {
    最后执行的代码;
}
```

示例：

```java
public class FinallyDemo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
            System.out.println(result);
        } catch (ArithmeticException e) {
            System.out.println("除数不能为 0");
        } finally {
            System.out.println("finally 执行");
        }

        System.out.println("程序结束");
    }
}
```

运行后可以观察：即使 `try` 中出现异常，`finally` 仍会执行。

### 7.2 `finally` 的常见用途

`finally` 常用于关闭资源。

例如：

```java
import java.io.FileInputStream;
import java.io.IOException;

public class FinallyCloseDemo {
    public static void main(String[] args) {
        FileInputStream input = null;

        try {
            input = new FileInputStream("data.txt");
            int value = input.read();
            System.out.println(value);
        } catch (IOException e) {
            System.out.println("文件读取失败");
        } finally {
            if (input != null) {
                try {
                    input.close();
                } catch (IOException e) {
                    System.out.println("文件关闭失败");
                }
            }
        }
    }
}
```

这段代码较繁琐。后面会学习 `try-with-resources`，它可以自动关闭资源。

### 7.3 `finally` 与 `return`

即使 `try` 或 `catch` 中出现 `return`，`finally` 通常也会执行。

```java
public class FinallyReturnDemo {
    public static int getNumber() {
        try {
            return 10;
        } finally {
            System.out.println("finally 执行");
        }
    }

    public static void main(String[] args) {
        int number = getNumber();
        System.out.println(number);
    }
}
```

基础阶段应记住：`finally` 主要用于清理资源，不建议在 `finally` 中写复杂业务逻辑。

## 8. `throw` 和 `throws`

### 8.1 `throw`

`throw` 用于在方法内部主动抛出一个异常对象。

```java
public class ThrowDemo {
    public static void checkAge(int age) {
        if (age < 0 || age > 120) {
            throw new IllegalArgumentException("年龄不合法");
        }

        System.out.println("年龄合法");
    }

    public static void main(String[] args) {
        checkAge(150);
    }
}
```

其中：

1. `new IllegalArgumentException("年龄不合法")` 创建异常对象。
2. `throw` 把这个异常对象抛出。
3. 方法执行到 `throw` 后，后续普通代码不会继续执行。

`throw` 常用于参数校验。

### 8.2 `throws`

`throws` 写在方法声明上，表示这个方法可能抛出某种异常。

```java
import java.io.FileInputStream;
import java.io.FileNotFoundException;

public class ThrowsDemo {
    public static void readFile() throws FileNotFoundException {
        FileInputStream input = new FileInputStream("data.txt");
    }

    public static void main(String[] args) throws FileNotFoundException {
        readFile();
    }
}
```

`throws` 的含义是：当前方法不在这里处理异常，而是把异常交给调用者处理。

### 8.3 `throw` 与 `throws` 的区别

| 对比项 | `throw` | `throws` |
| ------ | ------- | -------- |
| 位置 | 方法体内部 | 方法声明上 |
| 作用 | 主动抛出一个异常对象 | 声明方法可能抛出异常 |
| 后面内容 | 一个异常对象 | 一个或多个异常类型 |
| 示例 | `throw new RuntimeException()` | `throws IOException` |

示例：

```java
public static void checkName(String name) throws IllegalArgumentException {
    if (name == null || name.length() == 0) {
        throw new IllegalArgumentException("姓名不能为空");
    }
}
```

其中：

1. `throws IllegalArgumentException` 声明方法可能抛出参数异常。
2. `throw new IllegalArgumentException(...)` 是真正抛出异常对象的代码。

运行时异常可以不写 `throws`，上述示例只是为了展示语法。

## 9. 自定义异常

### 9.1 为什么需要自定义异常

Java 提供了很多异常类型，但有时业务问题需要更明确的异常名称。

例如：

1. 年龄不合法。
2. 库存不足。
3. 成绩不合法。
4. 账户余额不足。

这些问题都可以用通用异常表示，但自定义异常能让代码含义更清楚。

### 9.2 自定义运行时异常

自定义运行时异常通常继承 `RuntimeException`。

```java
public class InvalidScoreException extends RuntimeException {
    public InvalidScoreException() {
    }

    public InvalidScoreException(String message) {
        super(message);
    }
}
```

使用示例：

```java
public class ScoreService {
    public static void checkScore(int score) {
        if (score < 0 || score > 100) {
            throw new InvalidScoreException("成绩必须在 0 到 100 之间");
        }

        System.out.println("成绩合法");
    }
}
```

测试：

```java
public class CustomRuntimeExceptionDemo {
    public static void main(String[] args) {
        ScoreService.checkScore(120);
    }
}
```

### 9.3 自定义受检异常

自定义受检异常通常继承 `Exception`。

```java
public class BalanceNotEnoughException extends Exception {
    public BalanceNotEnoughException() {
    }

    public BalanceNotEnoughException(String message) {
        super(message);
    }
}
```

使用示例：

```java
public class Account {
    private double balance;

    public Account(double balance) {
        this.balance = balance;
    }

    public void withdraw(double money) throws BalanceNotEnoughException {
        if (money > balance) {
            throw new BalanceNotEnoughException("余额不足");
        }

        balance -= money;
    }

    public double getBalance() {
        return balance;
    }
}
```

测试：

```java
public class CustomCheckedExceptionDemo {
    public static void main(String[] args) {
        Account account = new Account(100);

        try {
            account.withdraw(200);
        } catch (BalanceNotEnoughException e) {
            System.out.println(e.getMessage());
        }
    }
}
```

自定义受检异常会强制调用者处理或继续抛出。

### 9.4 自定义异常选择

基础阶段可以按以下方式选择：

1. 如果问题通常来自代码参数或调用方式错误，可以继承 `RuntimeException`。
2. 如果调用者必须明确处理这个问题，可以继承 `Exception`。
3. 自定义异常类名通常以 `Exception` 结尾。
4. 异常信息应清楚说明问题原因。

## 10. 异常处理原则

### 10.1 不要随意吞掉异常

错误示例：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (Exception e) {
}
```

这种写法捕获了异常却什么都不做，会让问题变得难以发现。

更好的写法：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (NumberFormatException e) {
    System.out.println("数字格式不正确：" + e.getMessage());
}
```

### 10.2 捕获具体异常

能捕获具体异常时，不要一上来就捕获 `Exception`。

较好的写法：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (NumberFormatException e) {
    System.out.println("数字格式不正确");
}
```

不推荐的写法：

```java
try {
    int number = Integer.parseInt("abc");
    System.out.println(number);
} catch (Exception e) {
    System.out.println("出现异常");
}
```

捕获具体异常可以让程序员更清楚当前处理的是什么问题。

### 10.3 能预防的问题优先预防

有些运行时异常可以通过判断条件预防。

```java
public class PreventExceptionDemo {
    public static void main(String[] args) {
        int[] numbers = {10, 20, 30};
        int index = 2;

        if (index >= 0 && index < numbers.length) {
            System.out.println(numbers[index]);
        } else {
            System.out.println("索引不合法");
        }
    }
}
```

对于数组越界、空引用、参数不合法等问题，很多时候应通过条件判断或参数校验避免。

### 10.4 异常不是普通流程控制

异常用于处理非正常情况，不应把异常当作普通的 `if-else` 使用。

不推荐：

```java
try {
    int result = 10 / number;
    System.out.println(result);
} catch (ArithmeticException e) {
    System.out.println("除数不能为 0");
}
```

更适合基础业务判断的写法：

```java
if (number != 0) {
    int result = 10 / number;
    System.out.println(result);
} else {
    System.out.println("除数不能为 0");
}
```

## 11. IO 基础

### 11.1 什么是 IO

IO 是 Input 和 Output 的缩写。

Input 表示输入，即程序从外部读取数据。

Output 表示输出，即程序向外部写出数据。

常见 IO 场景：

| 场景 | 输入或输出 |
| ---- | ---------- |
| 从键盘读取用户输入 | 输入 |
| 在控制台打印文本 | 输出 |
| 从文件读取内容 | 输入 |
| 向文件写入内容 | 输出 |
| 从网络接收数据 | 输入 |
| 向网络发送数据 | 输出 |

本任务主要学习键盘、控制台和文件相关 IO。

### 11.2 流的含义

Java IO 中经常使用“流”这个概念。

流可以理解为数据传输的通道。数据像水流一样，从一个地方流向另一个地方。

```text
输入流：外部数据 -> Java 程序
输出流：Java 程序 -> 外部位置
```

按方向分类：

1. 输入流：读取数据。
2. 输出流：写出数据。

按数据单位分类：

1. 字节流：以字节为单位读写数据。
2. 字符流：以字符为单位读写文本。

### 11.3 字节流和字符流

字节流适合处理所有类型的数据，例如图片、音频、视频、压缩包、普通文本。

字符流适合处理文本数据，例如 `.txt`、`.java`、`.md`。

常见父类：

| 类型 | 输入 | 输出 |
| ---- | ---- | ---- |
| 字节流 | `InputStream` | `OutputStream` |
| 字符流 | `Reader` | `Writer` |

常见实现类：

| 类 | 含义 |
| -- | ---- |
| `FileInputStream` | 文件字节输入流 |
| `FileOutputStream` | 文件字节输出流 |
| `FileReader` | 文件字符输入流 |
| `FileWriter` | 文件字符输出流 |
| `BufferedInputStream` | 缓冲字节输入流 |
| `BufferedOutputStream` | 缓冲字节输出流 |
| `BufferedReader` | 缓冲字符输入流 |
| `BufferedWriter` | 缓冲字符输出流 |

## 12. 标准输入输出

### 12.1 `System` 类

`System` 是 Java 提供的系统相关工具类，位于 `java.lang` 包中。

`java.lang` 包中的类会被自动导入，因此使用 `System` 时不需要手动写 `import`。

常见成员：

| 成员 | 类型 | 含义 |
| ---- | ---- | ---- |
| `System.in` | `InputStream` | 标准输入流，通常连接键盘 |
| `System.out` | `PrintStream` | 标准输出流，通常连接控制台 |
| `System.err` | `PrintStream` | 标准错误输出流，通常连接控制台 |

### 12.2 为什么打印时是 `System.out.println`

`System.out.println("Hello")` 可以拆开理解：

```text
System  .  out  .  println("Hello")
系统类     标准输出流   打印一行
```

其中：

1. `System` 是系统类。
2. `out` 是 `System` 类中的静态成员变量。
3. `out` 的类型是 `PrintStream`。
4. `println` 是 `PrintStream` 对象的方法。

也就是说，`System.out.println` 并不是一个很长的方法名，而是“通过 `System` 找到标准输出对象，再调用这个对象的 `println` 方法”。

示例：

```java
public class SystemOutDemo {
    public static void main(String[] args) {
        System.out.println("Hello");
        System.out.print("Java");
        System.out.print(" IO");
    }
}
```

`println` 会打印内容并换行；`print` 只打印内容，不自动换行。

### 12.3 `System.err`

`System.err` 是标准错误输出流。

```java
public class SystemErrDemo {
    public static void main(String[] args) {
        System.out.println("普通输出");
        System.err.println("错误输出");
    }
}
```

在一些开发工具或终端中，`System.err` 的输出可能会用不同颜色显示。它常用于输出错误信息。

### 12.4 为什么 `Scanner` 需要传入 `System.in`

`Scanner` 是一个用于扫描和解析输入数据的工具类。

创建 `Scanner` 时传入 `System.in`：

```java
Scanner scanner = new Scanner(System.in);
```

可以拆开理解：

```text
System.in      表示数据来源，通常是键盘
new Scanner(...) 表示创建一个扫描器，用来从这个来源读取数据
```

`Scanner` 本身不代表键盘。它只是一个“读取和解析工具”。它需要知道从哪里读取数据，所以要把输入源传给它。

示例：

```java
import java.util.Scanner;

public class ScannerSystemInDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入姓名：");
        String name = scanner.nextLine();

        System.out.println("请输入年龄：");
        int age = scanner.nextInt();

        System.out.println(name + " - " + age);
    }
}
```

其中：

1. `System.in` 表示从键盘读入字节数据。
2. `Scanner` 负责把这些字节解析成字符串、整数、小数等类型。
3. `nextLine` 读取一行字符串。
4. `nextInt` 读取一个整数。

### 12.5 `Scanner` 也可以读取其他来源

`Scanner` 不只能读取键盘，也可以读取字符串或文件。

读取字符串：

```java
import java.util.Scanner;

public class ScannerStringDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner("Alice 18 95.5");

        String name = scanner.next();
        int age = scanner.nextInt();
        double score = scanner.nextDouble();

        System.out.println(name);
        System.out.println(age);
        System.out.println(score);
    }
}
```

这说明 `Scanner` 是解析数据的工具，而 `System.in` 只是常见输入来源之一。

### 12.6 `Scanner` 的换行问题

混合使用 `nextInt` 和 `nextLine` 时，容易遇到换行符问题。

```java
import java.util.Scanner;

public class ScannerLineProblemDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入年龄：");
        int age = scanner.nextInt();

        System.out.println("请输入姓名：");
        String name = scanner.nextLine();

        System.out.println(name + " - " + age);
    }
}
```

输入年龄后按回车，`nextInt` 只读取数字，不读取后面的换行符。接着 `nextLine` 会直接读取这个剩余的换行符，导致姓名没有机会输入。

常见解决方式是在 `nextInt` 后补一次 `nextLine`：

```java
import java.util.Scanner;

public class ScannerLineFixDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入年龄：");
        int age = scanner.nextInt();
        scanner.nextLine();

        System.out.println("请输入姓名：");
        String name = scanner.nextLine();

        System.out.println(name + " - " + age);
    }
}
```

### 12.7 是否需要关闭 `Scanner`

如果 `Scanner` 读取的是文件，通常应该关闭。

如果 `Scanner` 读取的是 `System.in`，在简单练习程序中可以不手动关闭。因为关闭 `Scanner` 会连带关闭它包装的 `System.in`，后续如果还想继续从键盘读取数据，可能会受到影响。

示例：

```java
Scanner scanner = new Scanner(System.in);
```

基础练习中重点关注读取逻辑即可。文件 IO 中则要重点学习资源关闭。

## 13. Java 25 及之后的 `IO.println`

### 13.1 为什么出现 `IO.println`

传统 Java 入门程序常写作：

```java
public class Hello {
    public static void main(String[] args) {
        System.out.println("Hello");
    }
}
```

这对初学者来说包含很多暂时还没学过的概念：

1. `public`。
2. `class`。
3. `static`。
4. `String[] args`。
5. `System.out.println`。

为了降低入门阶段的语法负担，Java 在较新的版本中逐步引入了更简洁的入门写法。到 Java 25 及之后，可以使用更适合初学者的 `IO.println` 打印内容。

示例：

```java
void main() {
    IO.println("Hello");
}
```

这种写法让初学者可以更早关注变量、流程控制和方法调用，而不是一开始就被完整类结构和标准输出对象吓住。

### 13.2 `IO.println` 的基本含义

`IO.println` 可以理解为 Java 为初学者提供的简化输出入口。

```java
IO.println("Hello");
```

其中：

1. `IO` 是 Java 25 及之后提供的简化 IO 类。
2. `println` 是用于打印一行内容的静态方法。
3. 它的效果类似于 `System.out.println`。

传统写法：

```java
System.out.println("Hello");
```

简化写法：

```java
IO.println("Hello");
```

### 13.3 为什么可以直接使用 `IO`

`System` 来自 `java.lang` 包，`java.lang` 包会被自动导入，所以可以直接写 `System`。

Java 25 及之后的 `IO` 完整类名是 `java.lang.IO`。因为 `java.lang` 包会被自动导入，所以可以直接写 `IO.println`，不需要像普通工具类那样手动导入。

需要注意：

1. `IO.println` 需要较新的 JDK 支持。
2. 在 JDK 21 或更早版本中，通常仍应使用 `System.out.println`。
3. 学习 `IO.println` 不代表可以不理解 `System.out.println`。
4. 实际项目中仍然会大量看到 `System.out.println`、日志框架和各种流对象。

### 13.4 `IO.println` 与 `System.out.println` 的关系

可以把它们理解为不同层次的输出方式。

| 写法 | 特点 |
| ---- | ---- |
| `IO.println(...)` | 简洁，适合新版本 Java 入门程序 |
| `System.out.println(...)` | 传统，通用，适合理解标准输出流 |
| `PrintWriter`、`PrintStream` | 更灵活，适合文件、网络或其他输出目标 |

本任务仍然会重点讲 `System.out` 和流，因为它们是理解 Java IO 的基础。

## 14. `File`

### 14.1 什么是 `File`

`File` 表示文件或文件夹路径。

注意：`File` 对象本身不等于文件内容。它只是用来表示一个路径，并提供一些操作路径、判断文件状态的方法。

```java
import java.io.File;

public class FileDemo {
    public static void main(String[] args) {
        File file = new File("data.txt");

        System.out.println(file.getName());
        System.out.println(file.getAbsolutePath());
        System.out.println(file.exists());
    }
}
```

其中：

1. `new File("data.txt")` 创建一个表示 `data.txt` 路径的对象。
2. `getName()` 获取文件名。
3. `getAbsolutePath()` 获取绝对路径。
4. `exists()` 判断路径是否存在。

### 14.2 相对路径和绝对路径

绝对路径是从磁盘根目录开始的完整路径。

示例：

```text
D:\java-task\data.txt
```

相对路径是相对于当前程序运行目录的路径。

示例：

```text
data.txt
files\data.txt
```

在 IDEA 中运行程序时，相对路径通常相对于项目根目录。

### 14.3 `File` 常用方法

| 方法 | 含义 |
| ---- | ---- |
| `exists()` | 判断路径是否存在 |
| `isFile()` | 判断是否为文件 |
| `isDirectory()` | 判断是否为文件夹 |
| `getName()` | 获取名称 |
| `getAbsolutePath()` | 获取绝对路径 |
| `length()` | 获取文件大小，单位是字节 |
| `mkdir()` | 创建单级文件夹 |
| `mkdirs()` | 创建多级文件夹 |
| `delete()` | 删除文件或空文件夹 |
| `listFiles()` | 获取文件夹下的文件和文件夹 |

示例：

```java
import java.io.File;

public class FileMethodDemo {
    public static void main(String[] args) {
        File dir = new File("files");

        if (!dir.exists()) {
            dir.mkdirs();
        }

        System.out.println(dir.exists());
        System.out.println(dir.isDirectory());
        System.out.println(dir.getAbsolutePath());
    }
}
```

### 14.4 遍历文件夹

```java
import java.io.File;

public class ListFilesDemo {
    public static void main(String[] args) {
        File dir = new File(".");
        File[] files = dir.listFiles();

        if (files != null) {
            for (File file : files) {
                System.out.println(file.getName());
            }
        }
    }
}
```

`.` 表示当前目录。

`listFiles()` 可能返回 `null`，因此使用前应先判断。

## 15. 字节流

### 15.1 `InputStream` 和 `OutputStream`

字节流以字节为单位读写数据。

字节输入流父类：

```java
InputStream
```

字节输出流父类：

```java
OutputStream
```

常用文件字节流：

1. `FileInputStream`：从文件读取字节。
2. `FileOutputStream`：向文件写入字节。

字节流可以处理任意类型文件。

### 15.2 使用 `FileOutputStream` 写文件

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class FileOutputStreamDemo {
    public static void main(String[] args) {
        try (FileOutputStream output = new FileOutputStream("data.txt")) {
            output.write(65);
            output.write(66);
            output.write(67);
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

运行后，`data.txt` 中会写入 `ABC`。

其中：

1. `FileOutputStream` 用于写出字节。
2. `write(65)` 写入一个字节，`65` 对应字符 `A`。
3. `try-with-resources` 会自动关闭 `output`。

### 15.3 写入字节数组

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class WriteBytesDemo {
    public static void main(String[] args) {
        String text = "Hello Java";
        byte[] bytes = text.getBytes(StandardCharsets.UTF_8);

        try (FileOutputStream output = new FileOutputStream("data.txt")) {
            output.write(bytes);
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

`getBytes(StandardCharsets.UTF_8)` 可以把字符串按照 UTF-8 编码转换为字节数组。

### 15.4 追加写入

默认情况下，`FileOutputStream` 会覆盖原文件内容。

```java
new FileOutputStream("data.txt")
```

如果希望追加写入，可以传入第二个参数 `true`。

```java
new FileOutputStream("data.txt", true)
```

示例：

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class AppendBytesDemo {
    public static void main(String[] args) {
        try (FileOutputStream output = new FileOutputStream("data.txt", true)) {
            output.write("追加内容\n".getBytes(StandardCharsets.UTF_8));
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

### 15.5 使用 `FileInputStream` 读文件

```java
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamDemo {
    public static void main(String[] args) {
        try (FileInputStream input = new FileInputStream("data.txt")) {
            int value;

            while ((value = input.read()) != -1) {
                System.out.print((char) value);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

其中：

1. `read()` 每次读取一个字节。
2. 返回 `-1` 表示读取结束。
3. `(char) value` 将字节值临时转换为字符显示。

一个字节一个字节读取效率较低，后面会学习使用缓冲区。

### 15.6 使用字节数组读取

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.nio.charset.StandardCharsets;

public class ReadBytesDemo {
    public static void main(String[] args) {
        byte[] buffer = new byte[1024];

        try (FileInputStream input = new FileInputStream("data.txt")) {
            int length;

            while ((length = input.read(buffer)) != -1) {
                String text = new String(buffer, 0, length, StandardCharsets.UTF_8);
                System.out.print(text);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

其中：

1. `byte[] buffer = new byte[1024]` 创建字节缓冲区。
2. `input.read(buffer)` 一次读取多个字节。
3. `length` 表示本次实际读取到的字节数量。
4. `new String(buffer, 0, length, StandardCharsets.UTF_8)` 将本次读取到的字节转换为字符串。

## 16. 字符流

### 16.1 为什么需要字符流

字节流可以处理所有文件，但处理文本时，经常需要考虑字符编码。

字符流以字符为单位读写文本，更适合处理纯文本文件。

常见字符流：

1. `FileReader`：读取文本文件。
2. `FileWriter`：写入文本文件。

### 16.2 使用 `FileWriter` 写文本

```java
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterDemo {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("message.txt")) {
            writer.write("Hello Java\n");
            writer.write("你好，IO\n");
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

`FileWriter` 用于写文本。它会根据默认字符编码把字符转换为字节写入文件。

### 16.3 使用 `FileReader` 读文本

```java
import java.io.FileReader;
import java.io.IOException;

public class FileReaderDemo {
    public static void main(String[] args) {
        try (FileReader reader = new FileReader("message.txt")) {
            int value;

            while ((value = reader.read()) != -1) {
                System.out.print((char) value);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

`reader.read()` 每次读取一个字符，返回 `-1` 表示结束。

### 16.4 字符数组读取

```java
import java.io.FileReader;
import java.io.IOException;

public class ReadCharsDemo {
    public static void main(String[] args) {
        char[] buffer = new char[1024];

        try (FileReader reader = new FileReader("message.txt")) {
            int length;

            while ((length = reader.read(buffer)) != -1) {
                String text = new String(buffer, 0, length);
                System.out.print(text);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

字符数组可以减少读取次数，提高效率。

## 17. 缓冲流

### 17.1 什么是缓冲流

缓冲流会在普通流外面包一层缓冲区，从而减少直接访问磁盘的次数，提高读写效率。

常见缓冲流：

| 缓冲流 | 包装对象 |
| ------ | -------- |
| `BufferedInputStream` | 字节输入流 |
| `BufferedOutputStream` | 字节输出流 |
| `BufferedReader` | 字符输入流 |
| `BufferedWriter` | 字符输出流 |

缓冲流通常不是单独使用，而是包装已有的流。

```java
BufferedReader reader = new BufferedReader(new FileReader("message.txt"));
```

可以理解为：`FileReader` 负责从文件读取字符，`BufferedReader` 负责让读取更高效，并提供更方便的方法。

### 17.2 缓冲字节流复制文件

```java
import java.io.BufferedInputStream;
import java.io.BufferedOutputStream;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class BufferedCopyDemo {
    public static void main(String[] args) {
        try (
                BufferedInputStream input = new BufferedInputStream(new FileInputStream("source.dat"));
                BufferedOutputStream output = new BufferedOutputStream(new FileOutputStream("target.dat"))
        ) {
            byte[] buffer = new byte[8192];
            int length;

            while ((length = input.read(buffer)) != -1) {
                output.write(buffer, 0, length);
            }
        } catch (IOException e) {
            System.out.println("复制失败：" + e.getMessage());
        }
    }
}
```

这种方式适合复制任意类型文件。

### 17.3 `BufferedReader` 按行读取

```java
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class BufferedReaderDemo {
    public static void main(String[] args) {
        try (BufferedReader reader = new BufferedReader(new FileReader("message.txt"))) {
            String line;

            while ((line = reader.readLine()) != null) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

`readLine()` 每次读取一行文本，读到文件末尾时返回 `null`。

注意：`readLine()` 返回的字符串不包含换行符。

### 17.4 `BufferedWriter` 写文本

```java
import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferedWriterDemo {
    public static void main(String[] args) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter("message.txt"))) {
            writer.write("第一行");
            writer.newLine();
            writer.write("第二行");
            writer.newLine();
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

`newLine()` 会根据当前系统写入合适的换行符。

## 18. 转换流与字符编码

### 18.1 什么是字符编码

计算机底层保存的是字节。文本中的字符需要通过编码转换成字节保存，也需要通过编码把字节还原成字符。

常见编码：

1. UTF-8。
2. GBK。
3. UTF-16。

如果写入时使用一种编码，读取时使用另一种编码，可能会出现乱码。

### 18.2 `InputStreamReader`

`InputStreamReader` 可以把字节输入流转换为字符输入流，并指定字符编码。

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;

public class InputStreamReaderDemo {
    public static void main(String[] args) {
        try (
                InputStreamReader reader = new InputStreamReader(
                        new FileInputStream("message.txt"),
                        StandardCharsets.UTF_8
                )
        ) {
            int value;

            while ((value = reader.read()) != -1) {
                System.out.print((char) value);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

### 18.3 `OutputStreamWriter`

`OutputStreamWriter` 可以把字符输出流转换为字节输出流，并指定字符编码。

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;
import java.nio.charset.StandardCharsets;

public class OutputStreamWriterDemo {
    public static void main(String[] args) {
        try (
                OutputStreamWriter writer = new OutputStreamWriter(
                        new FileOutputStream("message.txt"),
                        StandardCharsets.UTF_8
                )
        ) {
            writer.write("使用 UTF-8 写入文本");
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

### 18.4 什么时候使用转换流

当需要明确指定文本编码时，可以使用转换流。

```java
BufferedReader reader = new BufferedReader(
        new InputStreamReader(
                new FileInputStream("message.txt"),
                StandardCharsets.UTF_8
        )
);
```

这段代码可以理解为：

1. `FileInputStream` 从文件读取字节。
2. `InputStreamReader` 按 UTF-8 把字节转换为字符。
3. `BufferedReader` 提供缓冲和按行读取能力。

## 19. 打印流

### 19.1 `PrintStream`

`System.out` 的类型就是 `PrintStream`。

`PrintStream` 提供了常见的 `print`、`println`、`printf` 方法。

```java
public class PrintStreamDemo {
    public static void main(String[] args) {
        System.out.print("Hello");
        System.out.println(" Java");
        System.out.printf("%s 的成绩是 %d%n", "Alice", 95);
    }
}
```

其中：

1. `print` 不自动换行。
2. `println` 自动换行。
3. `printf` 可以按照格式输出。

### 19.2 向文件打印

```java
import java.io.FileNotFoundException;
import java.io.PrintStream;
import java.nio.charset.StandardCharsets;

public class PrintToFileDemo {
    public static void main(String[] args) {
        try (PrintStream printStream = new PrintStream("report.txt", StandardCharsets.UTF_8)) {
            printStream.println("成绩单");
            printStream.println("Alice: 95");
            printStream.println("Bob: 88");
        } catch (FileNotFoundException e) {
            System.out.println("文件创建失败：" + e.getMessage());
        }
    }
}
```

`PrintStream` 可以把打印内容输出到文件，而不仅仅是控制台。

### 19.3 `PrintWriter`

`PrintWriter` 也是常用打印输出类，适合写文本。

```java
import java.io.IOException;
import java.io.PrintWriter;
import java.nio.charset.StandardCharsets;

public class PrintWriterDemo {
    public static void main(String[] args) {
        try (PrintWriter writer = new PrintWriter("report.txt", StandardCharsets.UTF_8)) {
            writer.println("商品列表");
            writer.printf("%s: %.2f%n", "Book", 39.9);
            writer.printf("%s: %.2f%n", "Pen", 5.5);
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

## 20. `try-with-resources`

### 20.1 为什么需要关闭资源

文件流、网络连接、数据库连接等资源使用后应关闭。

如果不关闭资源，可能出现以下问题：

1. 文件被长期占用。
2. 数据没有及时写入磁盘。
3. 系统资源被浪费。

传统方式可以在 `finally` 中关闭资源，但代码较繁琐。

### 20.2 `try-with-resources` 基本写法

`try-with-resources` 可以自动关闭资源。

```java
try (资源对象声明) {
    使用资源;
} catch (异常类型 变量名) {
    异常处理;
}
```

示例：

```java
import java.io.FileWriter;
import java.io.IOException;

public class TryWithResourcesDemo {
    public static void main(String[] args) {
        try (FileWriter writer = new FileWriter("message.txt")) {
            writer.write("自动关闭资源");
        } catch (IOException e) {
            System.out.println("写入失败：" + e.getMessage());
        }
    }
}
```

当 `try` 代码块执行结束后，`writer` 会被自动关闭。

### 20.3 多个资源

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class MultiResourceDemo {
    public static void main(String[] args) {
        try (
                FileInputStream input = new FileInputStream("source.txt");
                FileOutputStream output = new FileOutputStream("target.txt")
        ) {
            byte[] buffer = new byte[1024];
            int length;

            while ((length = input.read(buffer)) != -1) {
                output.write(buffer, 0, length);
            }
        } catch (IOException e) {
            System.out.println("复制失败：" + e.getMessage());
        }
    }
}
```

多个资源之间使用分号分隔。

### 20.4 哪些对象可以自动关闭

能够放在 `try-with-resources` 中的对象，通常需要实现 `AutoCloseable` 或 `Closeable` 接口。

常见可以自动关闭的资源：

1. `FileInputStream`。
2. `FileOutputStream`。
3. `FileReader`。
4. `FileWriter`。
5. `BufferedReader`。
6. `BufferedWriter`。
7. `PrintWriter`。
8. `Scanner`。

基础阶段可以简单理解：大多数 IO 流对象都可以放进 `try-with-resources` 中自动关闭。

## 21. 对象序列化

### 21.1 什么是序列化

对象序列化是把 Java 对象转换成字节数据并保存起来的过程。

反序列化是把字节数据还原成 Java 对象的过程。

```text
对象 -> 字节数据 -> 文件
文件 -> 字节数据 -> 对象
```

对象序列化可以用于保存对象状态。

### 21.2 `Serializable`

如果一个对象想被序列化，它所属的类通常需要实现 `Serializable` 接口。

```java
import java.io.Serializable;

public class Student implements Serializable {
    private static final long serialVersionUID = 1L;

    private String name;
    private int age;

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + "-" + age;
    }
}
```

`Serializable` 是一个标记接口，不需要实现方法。

`serialVersionUID` 用于表示序列化版本。基础阶段可以先按固定写法理解。

### 21.3 使用 `ObjectOutputStream` 写对象

```java
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class ObjectOutputDemo {
    public static void main(String[] args) {
        Student student = new Student("Alice", 18);

        try (ObjectOutputStream output = new ObjectOutputStream(new FileOutputStream("student.dat"))) {
            output.writeObject(student);
        } catch (IOException e) {
            System.out.println("对象写入失败：" + e.getMessage());
        }
    }
}
```

### 21.4 使用 `ObjectInputStream` 读对象

```java
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class ObjectInputDemo {
    public static void main(String[] args) {
        try (ObjectInputStream input = new ObjectInputStream(new FileInputStream("student.dat"))) {
            Object value = input.readObject();

            if (value instanceof Student) {
                Student student = (Student) value;
                System.out.println(student);
            }
        } catch (IOException e) {
            System.out.println("对象读取失败：" + e.getMessage());
        } catch (ClassNotFoundException e) {
            System.out.println("找不到对象所属的类：" + e.getMessage());
        }
    }
}
```

`readObject()` 的返回类型是 `Object`，因此通常需要类型判断和强制类型转换。

### 21.5 `transient`

如果某个成员变量不希望被序列化，可以使用 `transient` 修饰。

```java
import java.io.Serializable;

public class User implements Serializable {
    private static final long serialVersionUID = 1L;

    private String username;
    private transient String password;

    public User(String username, String password) {
        this.username = username;
        this.password = password;
    }

    @Override
    public String toString() {
        return username + "-" + password;
    }
}
```

`password` 被 `transient` 修饰后，不会参与序列化。

## 22. `Path` 与 `Files`

### 22.1 `Path`

`Path` 是 `java.nio.file` 包中用于表示路径的类型。

相比传统 `File`，`Path` 和 `Files` 提供了更现代、更方便的文件操作方式。

```java
import java.nio.file.Path;

public class PathDemo {
    public static void main(String[] args) {
        Path path = Path.of("message.txt");

        System.out.println(path.getFileName());
        System.out.println(path.toAbsolutePath());
    }
}
```

### 22.2 `Files` 常用方法

`Files` 是文件工具类，提供大量静态方法。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `exists(Path path)` | 判断路径是否存在 |
| `isRegularFile(Path path)` | 判断是否为普通文件 |
| `isDirectory(Path path)` | 判断是否为文件夹 |
| `createDirectories(Path path)` | 创建多级文件夹 |
| `readString(Path path)` | 读取整个文本文件 |
| `writeString(Path path, CharSequence csq)` | 写入字符串 |
| `readAllLines(Path path)` | 读取所有行 |
| `copy(Path source, Path target)` | 复制文件 |
| `deleteIfExists(Path path)` | 如果存在则删除 |

### 22.3 使用 `Files` 写入和读取字符串

```java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class FilesStringDemo {
    public static void main(String[] args) {
        Path path = Path.of("note.txt");

        try {
            Files.writeString(path, "Hello Files\n你好 Java", StandardCharsets.UTF_8);

            String content = Files.readString(path, StandardCharsets.UTF_8);
            System.out.println(content);
        } catch (IOException e) {
            System.out.println("文件操作失败：" + e.getMessage());
        }
    }
}
```

`Files.readString` 和 `Files.writeString` 适合处理小文本文件。

### 22.4 读取所有行

```java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.List;

public class FilesLinesDemo {
    public static void main(String[] args) {
        Path path = Path.of("note.txt");

        try {
            List<String> lines = Files.readAllLines(path, StandardCharsets.UTF_8);

            for (String line : lines) {
                System.out.println(line);
            }
        } catch (IOException e) {
            System.out.println("读取失败：" + e.getMessage());
        }
    }
}
```

`readAllLines` 会把所有行读入 `List<String>`。

### 22.5 复制文件

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.nio.file.StandardCopyOption;

public class FilesCopyDemo {
    public static void main(String[] args) {
        Path source = Path.of("note.txt");
        Path target = Path.of("note-copy.txt");

        try {
            Files.copy(source, target, StandardCopyOption.REPLACE_EXISTING);
        } catch (IOException e) {
            System.out.println("复制失败：" + e.getMessage());
        }
    }
}
```

`StandardCopyOption.REPLACE_EXISTING` 表示目标文件已存在时覆盖。

## 23. 代码示例

在 IDEA 中创建以下文件，并保证它们位于同一个包或同一个源码目录下。

### 23.1 `InvalidBookException.java`

```java
public class InvalidBookException extends RuntimeException {
    public InvalidBookException() {
    }

    public InvalidBookException(String message) {
        super(message);
    }
}
```

### 23.2 `Book.java`

```java
import java.io.Serializable;
import java.math.BigDecimal;
import java.util.Objects;

public class Book implements Serializable {
    private static final long serialVersionUID = 1L;

    private String name;
    private String author;
    private BigDecimal price;

    public Book(String name, String author, BigDecimal price) {
        if (name == null || name.length() == 0) {
            throw new InvalidBookException("书名不能为空");
        }

        if (author == null || author.length() == 0) {
            throw new InvalidBookException("作者不能为空");
        }

        if (price == null || price.compareTo(BigDecimal.ZERO) < 0) {
            throw new InvalidBookException("价格不能为负数");
        }

        this.name = name;
        this.author = author;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public String getAuthor() {
        return author;
    }

    public BigDecimal getPrice() {
        return price;
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
        return Objects.equals(name, other.name)
                && Objects.equals(author, other.author);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, author);
    }

    @Override
    public String toString() {
        return name + "-" + author + "-" + price;
    }
}
```

### 23.3 `Task5Demo.java`

```java
import java.io.BufferedReader;
import java.io.BufferedWriter;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.OutputStreamWriter;
import java.math.BigDecimal;
import java.nio.charset.StandardCharsets;
import java.util.ArrayList;

public class Task5Demo {
    public static void saveBooks(ArrayList<Book> books, String fileName) throws IOException {
        try (
                BufferedWriter writer = new BufferedWriter(
                        new OutputStreamWriter(
                                new FileOutputStream(fileName),
                                StandardCharsets.UTF_8
                        )
                )
        ) {
            for (Book book : books) {
                writer.write(book.getName() + "," + book.getAuthor() + "," + book.getPrice());
                writer.newLine();
            }
        }
    }

    public static ArrayList<String> readLines(String fileName) throws IOException {
        ArrayList<String> lines = new ArrayList<>();

        try (
                BufferedReader reader = new BufferedReader(
                        new InputStreamReader(
                                new FileInputStream(fileName),
                                StandardCharsets.UTF_8
                        )
                )
        ) {
            String line;

            while ((line = reader.readLine()) != null) {
                lines.add(line);
            }
        }

        return lines;
    }

    public static void saveObject(Object value, String fileName) throws IOException {
        try (ObjectOutputStream output = new ObjectOutputStream(new FileOutputStream(fileName))) {
            output.writeObject(value);
        }
    }

    public static Object readObject(String fileName) throws IOException, ClassNotFoundException {
        try (ObjectInputStream input = new ObjectInputStream(new FileInputStream(fileName))) {
            return input.readObject();
        }
    }

    public static void main(String[] args) {
        try {
            ArrayList<Book> books = new ArrayList<>();
            books.add(new Book("Java 基础", "Alice", new BigDecimal("59.90")));
            books.add(new Book("集合与泛型", "Bob", new BigDecimal("49.90")));
            books.add(new Book("异常与 IO", "Cindy", new BigDecimal("69.90")));

            saveBooks(books, "books.txt");

            ArrayList<String> lines = readLines("books.txt");

            for (String line : lines) {
                System.out.println(line);
            }

            saveObject(books, "books.dat");

            Object value = readObject("books.dat");

            if (value instanceof ArrayList<?>) {
                System.out.println(value);
            }
        } catch (InvalidBookException e) {
            System.out.println("图书数据不合法：" + e.getMessage());
        } catch (IOException e) {
            System.out.println("文件操作失败：" + e.getMessage());
        } catch (ClassNotFoundException e) {
            System.out.println("对象读取失败：" + e.getMessage());
        }
    }
}
```

运行后应重点观察以下内容：

1. `InvalidBookException` 如何表示自定义业务异常。
2. `Book` 构造方法中如何使用 `throw` 主动抛出异常。
3. `saveBooks` 方法为什么使用 `throws IOException`。
4. `try-with-resources` 如何自动关闭流。
5. `BufferedWriter` 如何按行写入文本。
6. `BufferedReader` 如何按行读取文本。
7. `OutputStreamWriter` 和 `InputStreamReader` 如何指定 UTF-8 编码。
8. `ObjectOutputStream` 如何写出对象。
9. `ObjectInputStream` 如何读取对象。
10. `main` 方法如何使用多个 `catch` 处理不同异常。

## 24. 练习

### 24.1 异常基础练习

创建 `ExceptionPractice.java`，完成以下要求：

1. 编写数组索引越界示例，并使用 `try-catch` 捕获。
2. 编写字符串转整数失败示例，并捕获 `NumberFormatException`。
3. 编写除数为 `0` 示例，并捕获 `ArithmeticException`。
4. 在每个 `catch` 中输出异常提示信息。
5. 使用 `getMessage` 输出异常信息。
6. 使用至少一个多个 `catch` 的示例。
7. 写注释说明运行时异常和受检异常的区别。

### 24.2 `throw`、`throws` 与自定义异常练习

创建 `ScoreException.java` 和 `ScoreService.java`，完成以下要求：

1. 定义 `ScoreException`，继承 `RuntimeException`。
2. 为 `ScoreException` 定义无参构造方法和带字符串参数的构造方法。
3. 在 `ScoreService` 中定义 `checkScore(int score)` 方法。
4. 当成绩小于 `0` 或大于 `100` 时，使用 `throw` 抛出 `ScoreException`。
5. 成绩合法时输出 `成绩合法`。
6. 创建测试类，调用 `checkScore`，并使用 `try-catch` 捕获异常。
7. 写注释说明 `throw` 和 `throws` 的区别。

### 24.3 标准输入输出练习

创建 `StandardIOPractice.java`，完成以下要求：

1. 使用 `System.out.println` 输出普通信息。
2. 使用 `System.err.println` 输出错误信息。
3. 使用 `Scanner scanner = new Scanner(System.in);` 读取姓名和年龄。
4. 使用注释说明 `System.in` 表示什么。
5. 使用注释说明 `System.out` 表示什么。
6. 演示 `nextInt` 后接 `nextLine` 的换行问题。
7. 使用补充一次 `nextLine` 的方式解决问题。
8. 如果本机 JDK 支持 Java 25 或更新版本，尝试使用 `IO.println` 输出一行文本，并在注释中说明它和 `System.out.println` 的关系。

### 24.4 `File` 练习

创建 `FilePractice.java`，完成以下要求：

1. 创建 `File` 对象表示 `files` 文件夹。
2. 判断该文件夹是否存在。
3. 如果不存在，使用 `mkdirs` 创建。
4. 输出文件夹名称。
5. 输出文件夹绝对路径。
6. 判断它是否为文件夹。
7. 使用 `listFiles` 遍历当前目录下的文件和文件夹。
8. 对 `listFiles` 的返回值进行非空判断。

### 24.5 字节流练习

创建 `ByteStreamPractice.java`，完成以下要求：

1. 使用 `FileOutputStream` 向 `byte-data.txt` 写入 `ABC`。
2. 使用字符串的 `getBytes(StandardCharsets.UTF_8)` 写入一段中文文本。
3. 使用追加模式再次写入一行内容。
4. 使用 `FileInputStream` 读取 `byte-data.txt`。
5. 使用字节数组作为缓冲区读取内容。
6. 将读取到的字节转换为字符串输出。
7. 使用 `try-with-resources` 自动关闭流。
8. 使用 `catch` 捕获 `IOException`。

### 24.6 字符流与缓冲流练习

创建 `CharStreamPractice.java`，完成以下要求：

1. 使用 `FileWriter` 向 `char-data.txt` 写入多行文本。
2. 使用 `FileReader` 读取文本内容。
3. 使用 `BufferedWriter` 向 `buffer-data.txt` 写入多行文本。
4. 使用 `newLine` 写入换行。
5. 使用 `BufferedReader` 按行读取 `buffer-data.txt`。
6. 使用 `readLine` 判断是否读取结束。
7. 使用 `try-with-resources` 自动关闭资源。
8. 使用 `catch` 捕获 `IOException`。

### 24.7 编码与打印流练习

创建 `EncodingPrintPractice.java`，完成以下要求：

1. 使用 `OutputStreamWriter` 按 UTF-8 向 `utf8.txt` 写入中文文本。
2. 使用 `InputStreamReader` 按 UTF-8 读取 `utf8.txt`。
3. 使用 `PrintStream` 向 `print-report.txt` 写入多行内容。
4. 使用 `PrintWriter` 向 `writer-report.txt` 写入多行内容。
5. 使用 `printf` 格式化输出姓名和成绩。
6. 写注释说明为什么编码不一致可能会导致乱码。

### 24.8 序列化练习

创建 `Student.java` 和 `SerializationPractice.java`，完成以下要求：

1. `Student` 实现 `Serializable` 接口。
2. 定义 `serialVersionUID`。
3. 定义 `name`、`age`、`password` 三个成员变量。
4. 使用 `transient` 修饰 `password`。
5. 定义构造方法和 `toString` 方法。
6. 使用 `ObjectOutputStream` 将 `Student` 对象写入 `student.dat`。
7. 使用 `ObjectInputStream` 从 `student.dat` 读取对象。
8. 使用 `instanceof` 判断读取结果是否为 `Student`。
9. 输出读取到的对象，观察 `password` 的值。
10. 捕获 `IOException` 和 `ClassNotFoundException`。

### 24.9 `Files` 练习

创建 `FilesPractice.java`，完成以下要求：

1. 使用 `Path.of` 创建路径对象。
2. 使用 `Files.createDirectories` 创建文件夹。
3. 使用 `Files.writeString` 写入文本。
4. 使用 `Files.readString` 读取文本。
5. 使用 `Files.readAllLines` 读取所有行。
6. 使用 `Files.exists` 判断文件是否存在。
7. 使用 `Files.copy` 复制文件。
8. 使用 `StandardCopyOption.REPLACE_EXISTING` 覆盖已有文件。
9. 使用 `try-catch` 捕获 `IOException`。

### 24.10 综合练习

创建 `Task5Practice.java`，完成以下要求：

1. 定义 `Product` 类，包含 `name`、`price`、`stock`。
2. `Product` 实现 `Serializable` 接口。
3. 定义自定义异常 `InvalidProductException`。
4. 当商品名称为空、价格为负数或库存为负数时，抛出 `InvalidProductException`。
5. 创建 `ArrayList<Product>` 保存至少 5 个商品。
6. 使用 `BufferedWriter` 将商品信息写入 `products.txt`。
7. 使用 `BufferedReader` 按行读取 `products.txt` 并输出。
8. 使用 `ObjectOutputStream` 将商品集合写入 `products.dat`。
9. 使用 `ObjectInputStream` 读取 `products.dat`。
10. 使用 `Files.copy` 复制 `products.txt` 为 `products-copy.txt`。
11. 使用 `try-with-resources` 管理所有流资源。
12. 使用多个 `catch` 分别处理业务异常、IO 异常和类找不到异常。
13. 使用注释说明 `System.out.println`、`Scanner(System.in)` 和 `IO.println` 的基本含义。

## 25. 验收标准

满足以下条件视为完成 Task 5：

1. 能说明异常的含义。
2. 能说明 `Throwable`、`Error`、`Exception` 的基本关系。
3. 能区分运行时异常和受检异常。
4. 能识别常见运行时异常。
5. 能使用 `try-catch` 捕获并处理异常。
6. 能使用多个 `catch` 处理不同异常。
7. 能说明父类异常和子类异常的捕获顺序。
8. 能说明 `finally` 的作用。
9. 能使用 `throw` 主动抛出异常。
10. 能使用 `throws` 声明方法可能抛出的异常。
11. 能说明 `throw` 和 `throws` 的区别。
12. 能定义并使用自定义异常。
13. 能说明异常处理的基本原则。
14. 能说明 IO、输入、输出和流的含义。
15. 能说明 `System.in`、`System.out`、`System.err` 的作用。
16. 能说明为什么 `Scanner` 通常传入 `System.in`。
17. 能说明为什么打印通常使用 `System.out.println`。
18. 能说明 Java 25 及之后 `IO.println` 的简化意义。
19. 能使用 `File` 表示文件或文件夹路径。
20. 能使用字节流读写文件。
21. 能使用字符流读写文本文件。
22. 能使用缓冲流提高读写效率并按行读取文本。
23. 能使用转换流指定字符编码。
24. 能使用打印流输出格式化文本。
25. 能使用 `try-with-resources` 自动关闭资源。
26. 能使用对象序列化保存和读取对象。
27. 能使用 `Path` 和 `Files` 完成基础文件操作。
28. 能独立完成 `Task5Practice.java` 并运行得到正确结果。

## 26. 提交规范

1. 学习/实验总结报告统一命名为 `Task_5_report.md`，放置在 `Task_5/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_5/src/` 目录下（或在 `Task_5/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_5/` 目录中的本阶段内容。
