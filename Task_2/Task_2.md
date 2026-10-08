# Task 2：Java 流程控制与数组类型

## 1. 任务说明

本任务围绕 Java 程序执行顺序展开，重点包括使用 `Scanner` 获取输入、选择语句、循环语句、循环控制关键字以及数组类型。

Java 程序默认按照代码书写顺序从上到下执行。流程控制语句用于改变这种默认执行顺序，使程序能够根据条件选择不同分支，或者重复执行某段代码。数组用于保存一组类型相同的数据，是 Java 中最基础的批量数据结构。

本任务重点是掌握语句结构、执行规则和常见边界问题。

## 2. 知识范围

本任务包含以下内容：

1. 程序执行顺序与代码块。
2. 使用 `Scanner` 获取输入。
3. 选择语句。
4. 循环语句。
5. 循环控制关键字。
6. 数组类型。

## 3. 程序执行顺序与代码块

### 3.1 顺序执行

Java 程序中的语句默认从上到下依次执行。

```java
System.out.println("A");
System.out.println("B");
System.out.println("C");
```

输出结果：

```text
A
B
C
```

如果没有选择语句、循环语句、方法调用等改变执行顺序的结构，程序会按照语句出现的先后顺序执行。

### 3.2 代码块

代码块使用一对大括号 `{}` 表示。

```java
{
    int value = 10;
    System.out.println(value);
}
```

选择语句和循环语句通常都会使用代码块包裹其控制范围。

```java
if (score >= 60) {
    System.out.println("及格");
}
```

代码块也会影响变量作用域。定义在代码块内部的局部变量，只能在该代码块内部使用。

```java
if (true) {
    int value = 10;
    System.out.println(value);
}

// System.out.println(value); // 错误：value 的作用域只在上面的代码块内
```

### 3.3 条件表达式

选择语句和循环语句中的条件表达式结果必须是 `boolean` 类型。

```java
int age = 18;

if (age >= 18) {
    System.out.println("已成年");
}
```

`age >= 18` 的结果是 `true` 或 `false`，因此可以作为条件。

Java 中不能用整数直接充当条件。

```java
int value = 1;

// if (value) {       // 错误：Java 条件必须是 boolean
//     System.out.println("valid");
// }
```

## 4. 使用 `Scanner` 获取输入

命令行程序可以使用 `Scanner` 读取键盘输入。本任务只要求掌握 `Scanner` 的基础使用方式。

### 4.1 导入 `Scanner`

`Scanner` 不在默认导入范围内，使用前需要在文件顶部添加导入语句。

```java
import java.util.Scanner;
```

导入语句应写在 `public class` 之前。

```java
import java.util.Scanner;

public class InputDemo {
    public static void main(String[] args) {
        // 代码写在这里
    }
}
```

### 4.2 创建 `Scanner` 对象

在 `main` 方法中创建 `Scanner` 对象。

```java
Scanner scanner = new Scanner(System.in);
```

初学阶段可以先将该写法作为固定写法记住，用于从键盘读取输入。

### 4.3 读取常见类型

常用读取方法如下：

| 方法 | 读取内容 |
| --- | --- |
| `nextInt()` | 读取一个 `int` 整数 |
| `nextDouble()` | 读取一个 `double` 小数 |
| `next()` | 读取一个不包含空格的字符串 |
| `nextLine()` | 读取一整行字符串 |

示例：

```java
import java.util.Scanner;

public class InputDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入姓名：");
        String name = scanner.next();

        System.out.println("请输入年龄：");
        int age = scanner.nextInt();

        System.out.println("请输入成绩：");
        double score = scanner.nextDouble();

        System.out.println("姓名：" + name);
        System.out.println("年龄：" + age);
        System.out.println("成绩：" + score);
    }
}
```

输入类型应与读取方法匹配。例如，`nextInt()` 需要输入整数，`nextDouble()` 需要输入数值。如果输入内容无法转换为目标类型，程序会运行报错。

### 4.4 使用输入配合流程控制

读取到的数据可以作为选择语句或循环语句的判断依据。

```java
import java.util.Scanner;

public class ScoreInputDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入成绩：");
        int score = scanner.nextInt();

        if (score >= 60) {
            System.out.println("及格");
        } else {
            System.out.println("不及格");
        }
    }
}
```

## 5. 选择语句

选择语句用于根据条件决定执行哪一段代码。Java 中常见选择语句包括 `if`、`if-else`、`if-else if-else` 和 `switch`。

### 5.1 单分支 `if`

`if` 用于在条件成立时执行一段代码。

语法：

```text
if (条件表达式) {
    条件成立时执行的语句;
}
```

示例：

```java
int score = 85;

if (score >= 60) {
    System.out.println("及格");
}
```

当 `score >= 60` 为 `true` 时，执行代码块中的输出语句；为 `false` 时，跳过该代码块。

### 5.2 双分支 `if-else`

`if-else` 用于在两种情况中选择一种执行。

语法：

```text
if (条件表达式) {
    条件成立时执行的语句;
} else {
    条件不成立时执行的语句;
}
```

示例：

```java
int score = 58;

if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}
```

`if` 代码块和 `else` 代码块最多只会执行其中一个。

### 5.3 多分支 `if-else if-else`

`if-else if-else` 用于处理多个互斥条件。

语法：

```text
if (条件表达式1) {
    条件1成立时执行的语句;
} else if (条件表达式2) {
    条件2成立时执行的语句;
} else {
    以上条件都不成立时执行的语句;
}
```

示例：

```java
int score = 86;

if (score >= 90) {
    System.out.println("优秀");
} else if (score >= 80) {
    System.out.println("良好");
} else if (score >= 60) {
    System.out.println("及格");
} else {
    System.out.println("不及格");
}
```

多分支判断会从上到下依次检查条件。一旦某个条件成立并执行对应代码块，后续分支不再继续判断。

因此，区间判断通常应从范围较严格的条件开始写。

```java
int score = 95;

if (score >= 60) {
    System.out.println("及格");
} else if (score >= 90) {
    System.out.println("优秀");
}
```

上述代码中，`score >= 60` 已经成立，因此不会执行 `score >= 90` 对应分支。该写法无法正确区分优秀成绩。

### 5.4 嵌套 `if`

`if` 语句内部可以继续写 `if` 语句，这种结构称为嵌套 `if`。

```java
int score = 92;
boolean hasViolation = false;

if (score >= 90) {
    if (!hasViolation) {
        System.out.println("优秀且无违规记录");
    }
}
```

嵌套 `if` 适合表达需要同时满足多层条件的情况。但嵌套层级过多会降低可读性，初学阶段应尽量保持结构清晰。

### 5.5 `switch` 语句

`switch` 用于根据一个表达式的具体取值选择不同分支。它适合处理固定值匹配，而不是范围判断。

传统 `switch` 语法：

```text
switch (表达式) {
    case 值1:
        语句;
        break;
    case 值2:
        语句;
        break;
    default:
        语句;
        break;
}
```

示例：

```java
int day = 3;

switch (day) {
    case 1:
        System.out.println("Monday");
        break;
    case 2:
        System.out.println("Tuesday");
        break;
    case 3:
        System.out.println("Wednesday");
        break;
    default:
        System.out.println("Unknown");
        break;
}
```

执行过程如下：

1. 计算 `switch` 后括号中的表达式。
2. 从上到下查找匹配的 `case`。
3. 找到匹配项后执行对应语句。
4. 遇到 `break` 后结束整个 `switch`。
5. 如果没有任何 `case` 匹配，则执行 `default`。

`break` 用于防止继续向下执行其他 `case`。如果省略 `break`，程序会发生穿透。

```java
int day = 1;

switch (day) {
    case 1:
        System.out.println("Monday");
    case 2:
        System.out.println("Tuesday");
    default:
        System.out.println("Unknown");
}
```

输出结果：

```text
Monday
Tuesday
Unknown
```

这种情况称为 `case` 穿透。除非明确需要这种效果，否则每个 `case` 末尾通常应写 `break`。

传统 `switch` 常用于 `byte`、`short`、`char`、`int`、`String` 等类型的固定值判断。

```java
String command = "start";

switch (command) {
    case "start":
        System.out.println("启动");
        break;
    case "stop":
        System.out.println("停止");
        break;
    default:
        System.out.println("未知命令");
        break;
}
```

字符串内容比较在普通条件中应使用 `equals`；在 `switch` 中，Java 会根据字符串内容进行匹配。

## 6. 循环语句

循环语句用于重复执行一段代码。Java 中常见循环语句包括 `while`、`do-while` 和 `for`。

### 6.1 `while` 循环

`while` 循环先判断条件，再决定是否执行循环体。

语法：

```text
while (条件表达式) {
    循环体;
}
```

示例：

```java
int count = 1;

while (count <= 3) {
    System.out.println("count = " + count);
    count++;
}
```

输出结果：

```text
count = 1
count = 2
count = 3
```

执行过程如下：

1. 判断 `count <= 3`。
2. 如果条件为 `true`，执行循环体。
3. 执行 `count++`，改变循环变量。
4. 回到循环条件继续判断。
5. 当条件为 `false` 时，循环结束。

如果循环条件一开始就是 `false`，`while` 循环体一次也不会执行。

### 6.2 `do-while` 循环

`do-while` 循环先执行循环体，再判断条件。因此，循环体至少执行一次。

语法：

```text
do {
    循环体;
} while (条件表达式);
```

示例：

```java
int count = 5;

do {
    System.out.println("count = " + count);
    count++;
} while (count <= 3);
```

输出结果：

```text
count = 5
```

虽然 `count <= 3` 一开始就是 `false`，但 `do` 代码块已经先执行了一次。

### 6.3 `for` 循环

`for` 循环通常用于循环次数明确的场景。

语法：

```text
for (初始化语句; 条件表达式; 迭代语句) {
    循环体;
}
```

示例：

```java
for (int i = 1; i <= 3; i++) {
    System.out.println("i = " + i);
}
```

输出结果：

```text
i = 1
i = 2
i = 3
```

执行过程如下：

1. 执行初始化语句 `int i = 1`。
2. 判断条件表达式 `i <= 3`。
3. 条件成立时执行循环体。
4. 执行迭代语句 `i++`。
5. 回到条件表达式继续判断。
6. 条件不成立时结束循环。

### 6.4 循环边界

循环边界决定循环执行次数。边界条件写错，常见结果是少执行一次、多执行一次或死循环。

例如，输出 0 到 4：

```java
for (int i = 0; i < 5; i++) {
    System.out.println(i);
}
```

输出 1 到 5：

```java
for (int i = 1; i <= 5; i++) {
    System.out.println(i);
}
```

两种写法都正确，但含义不同。初学阶段应明确循环变量的起始值、结束条件和每次变化。

### 6.5 死循环

如果循环条件一直为 `true`，循环就不会自然结束。

```java
int count = 1;

while (count <= 3) {
    System.out.println(count);
    // count 没有变化，会导致死循环
}
```

避免死循环的关键是确保循环过程中条件会逐步接近结束状态。

```java
int count = 1;

while (count <= 3) {
    System.out.println(count);
    count++;
}
```

## 7. 循环控制关键字

循环控制关键字用于在循环执行过程中改变循环的正常执行流程。

### 7.1 `break`

`break` 用于立即结束当前循环。

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        break;
    }
    System.out.println(i);
}
```

输出结果：

```text
1
2
```

当 `i == 3` 时执行 `break`，整个 `for` 循环结束。

### 7.2 `continue`

`continue` 用于跳过本次循环剩余语句，进入下一次循环判断。

```java
for (int i = 1; i <= 5; i++) {
    if (i == 3) {
        continue;
    }
    System.out.println(i);
}
```

输出结果：

```text
1
2
4
5
```

当 `i == 3` 时执行 `continue`，本次循环中 `System.out.println(i)` 被跳过，循环继续执行下一次。

### 7.3 嵌套循环中的 `break` 与 `continue`

在嵌套循环中，普通 `break` 和 `continue` 只影响它所在的那一层循环。

```java
for (int i = 1; i <= 2; i++) {
    for (int j = 1; j <= 3; j++) {
        if (j == 2) {
            break;
        }
        System.out.println("i = " + i + ", j = " + j);
    }
}
```

输出结果：

```text
i = 1, j = 1
i = 2, j = 1
```

当 `j == 2` 时，结束的是内部 `for` 循环，外部 `for` 循环仍会继续执行。

## 8. 数组类型

### 8.1 数组的含义

数组用于保存一组类型相同的数据。数组中的每个数据称为元素，每个元素都有对应的索引。

示例：

```java
int[] scores = {90, 80, 70};
```

其中：

1. `int[]` 表示 `int` 类型数组。
2. `scores` 是数组变量名。
3. `{90, 80, 70}` 是数组中保存的元素。

数组是引用类型。数组变量保存的是数组对象的引用，而不是所有元素本身。

```java
int[] scores = {90, 80, 70};
```

上述代码中，`scores` 是引用类型变量，它指向一个保存了 3 个 `int` 元素的数组对象。

### 8.2 数组声明

数组声明的一般形式如下：

```text
元素类型[] 数组变量名;
```

示例：

```java
int[] scores;
double[] prices;
String[] names;
```

Java 也允许以下写法：

```java
int scores[];
```

但推荐使用 `int[] scores`。这种写法更清楚地表示变量类型是 `int[]`。

数组变量声明后，还没有指向具体数组对象。

```java
int[] scores;
```

局部数组变量在使用前必须完成赋值。

```java
int[] scores;
// System.out.println(scores.length); // 错误：局部变量 scores 尚未初始化
```

### 8.3 数组创建

数组创建使用 `new`。

```text
元素类型[] 数组变量名 = new 元素类型[长度];
```

示例：

```java
int[] scores = new int[3];
```

该语句创建了一个长度为 3 的 `int` 数组。数组长度在创建时确定，创建后不能改变。

数组创建后，每个元素会获得默认值。

| 元素类型 | 默认值 |
| --- | --- |
| 整数类型 | `0` |
| 浮点类型 | `0.0` |
| `char` | `'\u0000'` |
| `boolean` | `false` |
| 引用类型 | `null` |

示例：

```java
int[] scores = new int[3];

System.out.println(scores[0]); // 0
System.out.println(scores[1]); // 0
System.out.println(scores[2]); // 0
```

### 8.4 数组初始化

数组初始化可以在创建数组时直接指定元素。

静态初始化：

```java
int[] scores = {90, 80, 70};
String[] names = {"LiMing", "ZhangSan", "WangWu"};
```

完整写法：

```java
int[] scores = new int[] {90, 80, 70};
```

动态初始化：

```java
int[] scores = new int[3];
scores[0] = 90;
scores[1] = 80;
scores[2] = 70;
```

静态初始化适合已知全部元素值的场景；动态初始化适合先确定长度，再逐个赋值的场景。

### 8.5 数组索引

数组通过索引访问元素。Java 数组索引从 `0` 开始。

```java
int[] scores = {90, 80, 70};

System.out.println(scores[0]); // 90
System.out.println(scores[1]); // 80
System.out.println(scores[2]); // 70
```

长度为 3 的数组，合法索引是 `0`、`1`、`2`。最大索引是 `数组长度 - 1`。

可以通过索引修改数组元素：

```java
int[] scores = {90, 80, 70};
scores[1] = 85;

System.out.println(scores[1]); // 85
```

如果访问不存在的索引，会发生 `ArrayIndexOutOfBoundsException`。

```java
int[] scores = {90, 80, 70};

// System.out.println(scores[3]); // 运行时报错
```

### 8.6 数组长度

数组长度使用 `length` 获取。

```java
int[] scores = {90, 80, 70};

System.out.println(scores.length); // 3
```

注意，数组长度是 `length`，不是 `length()`。

```java
int[] scores = {90, 80, 70};

System.out.println(scores.length);   // 正确
// System.out.println(scores.length()); // 错误
```

`String` 获取长度使用 `length()` 方法，数组获取长度使用 `length` 属性。

```java
String text = "Java";
int[] scores = {90, 80, 70};

System.out.println(text.length());  // 4
System.out.println(scores.length);  // 3
```

### 8.7 数组遍历

数组遍历是指按照一定顺序访问数组中的每个元素。

使用 `for` 循环遍历数组：

```java
int[] scores = {90, 80, 70};

for (int i = 0; i < scores.length; i++) {
    System.out.println(scores[i]);
}
```

循环变量 `i` 从 `0` 开始，条件是 `i < scores.length`。这是数组遍历中最常用的边界写法。

使用增强 `for` 循环遍历数组：

```java
int[] scores = {90, 80, 70};

for (int score : scores) {
    System.out.println(score);
}
```

增强 `for` 适合只读取数组元素的场景。如果需要使用索引，或者需要根据索引修改数组元素，应使用普通 `for` 循环。

### 8.8 数组与引用赋值

数组是引用类型，数组变量赋值时复制的是引用。

```java
int[] first = {1, 2, 3};
int[] second = first;

second[0] = 100;

System.out.println(first[0]);  // 100
System.out.println(second[0]); // 100
```

`first` 和 `second` 指向同一个数组对象，因此通过 `second` 修改元素后，通过 `first` 也能看到修改结果。

如果创建两个独立数组，即使内容相同，也不是同一个数组对象。

```java
int[] a = {1, 2, 3};
int[] b = {1, 2, 3};

System.out.println(a == b); // false
```

`a == b` 比较的是两个数组变量是否指向同一个数组对象，而不是比较数组内容是否相同。

### 8.9 数组常见错误

#### 8.9.1 索引越界

```java
int[] scores = {90, 80, 70};

// System.out.println(scores[3]); // 错误：索引越界
```

长度为 3 的数组最大索引是 2。

#### 8.9.2 空引用

```java
int[] scores = null;

// System.out.println(scores.length); // 错误：NullPointerException
```

数组变量为 `null` 时，没有指向任何数组对象，不能访问元素或 `length`。

#### 8.9.3 循环边界错误

```java
int[] scores = {90, 80, 70};

for (int i = 0; i <= scores.length; i++) {
    // System.out.println(scores[i]); // 当 i 等于 3 时索引越界
}
```

数组正序遍历通常使用 `i < scores.length`，不是 `i <= scores.length`。

## 9. 代码示例

在 IDEA 中创建 `ControlArrayDemo.java`，输入以下代码并运行。

```java
import java.util.Scanner;

public class ControlArrayDemo {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        int[] scores = {95, 82, 76, 58, 100};

        int total = 0;
        int max = scores[0];
        int min = scores[0];
        int passCount = 0;

        for (int i = 0; i < scores.length; i++) {
            int score = scores[i];
            total += score;

            if (score > max) {
                max = score;
            }

            if (score < min) {
                min = score;
            }

            if (score >= 60) {
                passCount++;
            }

            String level;
            if (score >= 90) {
                level = "优秀";
            } else if (score >= 80) {
                level = "良好";
            } else if (score >= 60) {
                level = "及格";
            } else {
                level = "不及格";
            }

            System.out.println("第 " + (i + 1) + " 个成绩：" + score + "，等级：" + level);
        }

        double average = total * 1.0 / scores.length;

        System.out.println("总分：" + total);
        System.out.println("平均分：" + average);
        System.out.println("最高分：" + max);
        System.out.println("最低分：" + min);
        System.out.println("及格人数：" + passCount);

        System.out.println("请输入星期数字：");
        int day = scanner.nextInt();

        switch (day) {
            case 1:
                System.out.println("Monday");
                break;
            case 2:
                System.out.println("Tuesday");
                break;
            case 3:
                System.out.println("Wednesday");
                break;
            default:
                System.out.println("Unknown");
                break;
        }
    }
}
```

运行后应重点观察以下内容：

1. `for` 循环如何使用数组索引。
2. `scores.length` 如何控制循环边界。
3. `if-else if-else` 如何完成成绩等级判断。
4. `Scanner` 如何读取键盘输入。
5. `switch` 如何根据固定值选择分支。
6. `total`、`max`、`min`、`passCount` 如何在循环中逐步更新。

## 10. 练习

### 10.1 输入与选择语句练习

创建 `FlowArrayPractice.java`，完成以下要求：

1. 导入并创建 `Scanner`。
2. 使用 `Scanner` 读取一个 `int` 类型成绩。
3. 使用 `if-else if-else` 判断成绩等级。
4. 成绩大于等于 90 时输出 `优秀`。
5. 成绩大于等于 80 且小于 90 时输出 `良好`。
6. 成绩大于等于 60 且小于 80 时输出 `及格`。
7. 成绩小于 60 时输出 `不及格`。
8. 使用 `Scanner` 读取一个 `int` 类型星期数字。
9. 使用 `switch` 输出对应星期；无法匹配时输出 `Unknown`。

### 10.2 循环语句练习

在同一个文件中继续完成以下要求：

1. 使用 `for` 循环输出 `1` 到 `10`。
2. 使用 `while` 循环输出 `10` 到 `1`。
3. 使用 `do-while` 循环至少输出一次提示文本。
4. 使用 `continue` 跳过 `1` 到 `10` 中的偶数，只输出奇数。
5. 使用 `break` 在遇到第一个大于 `90` 的成绩时结束查找。

### 10.3 数组练习

在同一个文件中继续完成以下要求：

1. 声明并初始化数组 `int[] scores = {95, 82, 76, 58, 100};`。
2. 使用普通 `for` 循环遍历数组并输出每个元素。
3. 计算数组总分。
4. 计算数组平均分。
5. 找出最高分和最低分。
6. 统计及格人数。
7. 使用增强 `for` 循环再次输出所有成绩。
8. 写出一段注释，说明为什么数组遍历条件通常是 `i < scores.length`。

## 11. 验收标准

满足以下条件视为完成 Task 2：

1. 能说明顺序执行、选择执行和循环执行的区别。
2. 能使用 `Scanner` 读取基础输入。
3. 能正确书写 `if`、`if-else`、`if-else if-else`。
4. 能正确书写传统 `switch`，并说明 `case`、`break`、`default` 的作用。
5. 能正确书写 `while`、`do-while`、`for` 三种循环。
6. 能说明 `break` 与 `continue` 的区别。
7. 能说明数组是引用类型。
8. 能正确声明、创建和初始化数组。
9. 能通过索引访问和修改数组元素。
10. 能使用 `length` 获取数组长度。
11. 能使用循环遍历数组，并避免索引越界。
12. 能独立完成 `FlowArrayPractice.java` 并运行得到正确结果。

## 12. 提交规范

1. 学习/实验总结报告统一命名为 `Task_2_report.md`，放置在 `Task_2/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_2/src/` 目录下（或在 `Task_2/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_2/` 目录中的本阶段内容。
