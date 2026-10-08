# Task 1：Java 变量类型、字符串与基础运算规则

## 1. 任务说明

本任务围绕 Java 中最基础、最常用的数据表示方式展开，重点包括基本类型变量、引用类型变量、`String` 变量、运算符、字符串常量池以及基本类型参与运算时的隐式类型转换。

Java 是强类型语言。变量在声明时必须指定类型，变量类型决定了该变量能够保存什么数据、能够参与哪些运算、赋值时是否需要类型转换，以及比较时应使用何种方式。

## 2. 知识范围

本任务包含以下内容：

1. Java 基础程序结构。
2. 基本类型与引用类型变量概念。
3. 基本类型变量的分类与使用规则。
4. 运算符。
5. `String` 变量、字符串常量池与字符串比较。
6. 基本类型变量之间的类型转换。

## 3. Java 基础程序结构

Java 代码必须写在类中。初学阶段常见的可运行程序结构如下：

```java
public class VariableDemo {
    public static void main(String[] args) {
        System.out.println("Hello Java");
    }
}
```

其中：

1. `public class VariableDemo` 表示声明一个公开类，类名为 `VariableDemo`。
2. 如果一个类使用 `public` 修饰，源文件名必须与 `public class` 后面的类名完全相同，包括大小写。
3. 上述代码应保存为 `VariableDemo.java`。
4. `main` 方法是程序运行入口，初学阶段需要执行的语句通常写在 `main` 方法的大括号中。

方法是写在类中的一段具有名称的代码块，用于组织一组可以被执行的语句。Java 程序中的具体执行语句通常不会直接写在类的大括号中，而是写在某个方法的大括号中。

方法通常由方法声明和方法体组成。

```java
public static void main(String[] args) {
    System.out.println("Hello Java");
}
```

其中：

1. `main` 是方法名。
2. `public static void main(String[] args)` 是方法声明。
3. `{}` 中的内容是方法体。
4. `System.out.println("Hello Java");` 是写在方法体中的一条语句。

`main` 方法是 Java 程序的固定入口方法。初学阶段可以先将 `public static void main(String[] args)` 作为固定写法记住，重点理解程序运行时会从 `main` 方法的方法体开始执行。

Java 注释用于解释代码，不会被当作程序语句执行。常见注释写法如下：

```java
// 单行注释

/*
 多行注释
 可以写多行说明
*/

/**
 * 文档注释
 */
```

单行注释适合写简短说明；多行注释适合临时说明较长内容；文档注释常用于类、方法等结构的说明。

## 4. 变量的基本概念

### 4.1 变量声明

变量是程序中用于保存数据的命名存储单元。Java 中的变量必须先声明，再使用。

变量声明的一般形式如下：

```text
数据类型 变量名 = 初始值;
```

示例：

```java
int age = 18;
double price = 19.8;
String name = "ZhangSan";
```

其中：

1. `int`、`double`、`String` 是变量类型。
2. `age`、`price`、`name` 是变量名。
3. `18`、`19.8`、`"ZhangSan"` 是变量保存的初始数据。

### 4.2 变量命名规则

Java 变量名必须符合以下语法规则：

1. 变量名可以由字母、数字、下划线 `_` 和美元符号 `$` 组成。
2. 变量名不能以数字开头。
3. 变量名不能是 Java 关键字，如 `int`、`class`、`public`、`static`。
4. 变量名区分大小写，`age` 与 `Age` 是两个不同的变量名。
5. 变量名中不能包含空格。

合法变量名示例：

```java
int age = 18;
double totalScore = 276.5;
String studentName = "LiMing";
int count_1 = 10;
```

非法变量名示例：

```java
// int 1age = 18;       // 错误：不能以数字开头
// int student age = 18; // 错误：不能包含空格
// int class = 1;        // 错误：class 是关键字
```

在符合语法规则的基础上，变量命名还应具有可读性。Java 变量命名通常采用小驼峰风格，即首个单词小写，后续单词首字母大写。

示例：

```java
int studentAge = 18;
double totalScore = 276.5;
String studentName = "LiMing";
```

不推荐使用无意义的变量名：

```java
int a = 18;
double b = 276.5;
```

局部变量在使用前必须显式赋值。以下代码无法通过编译：

```java
int count;
System.out.println(count); // 错误：局部变量 count 尚未初始化
```

## 5. 基本类型变量

### 5.1 基本类型变量的含义

基本类型变量用于直接保存简单数据值。Java 中共有 8 种基本类型，分为整数类型、浮点类型、字符类型和布尔类型。

| 类型 | 分类 | 占用空间 | 说明 |
| --- | --- | ---: | --- |
| `byte` | 整数类型 | 1 字节 | 范围较小的整数 |
| `short` | 整数类型 | 2 字节 | 短整数 |
| `int` | 整数类型 | 4 字节 | 最常用的整数类型 |
| `long` | 整数类型 | 8 字节 | 范围更大的整数 |
| `float` | 浮点类型 | 4 字节 | 单精度浮点数 |
| `double` | 浮点类型 | 8 字节 | 双精度浮点数，默认小数类型 |
| `char` | 字符类型 | 2 字节 | 单个 Unicode 字符 |
| `boolean` | 布尔类型 | 未规定具体字节数 | 只能取 `true` 或 `false` |

示例：

```java
byte level = 3;
short year = 2026;
int age = 18;
long population = 8000000000L;
float height = 175.5F;
double score = 92.5;
char grade = 'A';
boolean passed = true;
```

### 5.2 整数类型

整数类型用于保存不带小数部分的数值。

```java
int count = 100;
long distance = 3000000000L;
```

整数直接量默认是 `int` 类型。当数值超出 `int` 范围，或者明确需要 `long` 类型时，应在数值末尾添加 `L`。

推荐使用大写 `L`，避免小写 `l` 与数字 `1` 混淆。

```java
long value = 10000000000L;
```

以下写法无法通过编译，因为右侧整数直接量默认按 `int` 处理，且数值超出 `int` 范围：

```java
long value = 10000000000; // 错误
```

整数类型具有固定取值范围。如果整数运算结果超出当前类型的取值范围，就会发生整数溢出。整数溢出不会自动转换为更大的类型，也不一定会在运行时报错，结果会按照该类型的二进制表示规则回绕。

例如，`int` 的最大值是 `2147483647`：

```java
int max = 2147483647;
int result = max + 1;

System.out.println(result); // -2147483648
```

`max + 1` 超出了 `int` 的最大取值范围，结果回绕为 `int` 的最小值。

如果运算结果可能超出 `int` 范围，应使用更大的整数类型参与运算，例如 `long`：

```java
int max = 2147483647;
long result = (long) max + 1;

System.out.println(result); // 2147483648
```

### 5.3 浮点类型

浮点类型用于保存带小数部分的数值。

```java
double price = 19.99;
float temperature = 36.5F;
```

小数直接量默认是 `double` 类型。因此，给 `float` 变量赋值时，通常需要在数值末尾添加 `F`。

```java
float rate = 0.85F;
```

以下写法无法通过编译：

```java
float rate = 0.85; // 错误：0.85 默认是 double
```

浮点数适合表示近似数值，不适合直接表示对精度要求极高的金额计算。金额计算在实际项目中通常使用 `BigDecimal`，但本任务只要求掌握 `float` 与 `double` 的基本规则。

### 5.4 字符类型

`char` 用于保存单个字符，字符值必须使用单引号。

```java
char gender = 'M';
char level = 'A';
char chinese = '中';
```

字符串必须使用双引号。`'A'` 是 `char`，`"A"` 是 `String`，二者类型不同。

```java
char c = 'A';
String s = "A";
```

`char` 本质上可以参与数值运算，运算时会转换为对应的 Unicode 编码值。

```java
char c = 'A';
int result = c + 1;
System.out.println(result); // 66
```

### 5.5 布尔类型

`boolean` 只有两个取值：

```java
boolean isAdult = true;
boolean hasError = false;
```

Java 中 `boolean` 不能与整数互相替代。`true` 不等于 `1`，`false` 不等于 `0`。

以下写法无法通过编译：

```java
boolean flag = 1; // 错误
int value = true; // 错误
```

## 6. 引用类型变量

### 6.1 引用类型变量的含义

引用类型变量保存的不是对象内容本身，而是对象的引用。可以将引用理解为程序找到某个对象的定位信息。

常见引用类型包括：

1. 类类型：如 `String`、`StringBuilder`、自定义类 `Student`。
2. 接口类型：如 `List`、`Map`。
3. 数组类型：如 `int[]`、`String[]`。

>数组会在后续任务中单独讲解。

引用类型变量可以只声明，不立即赋值：

```text
引用类型 变量名;
```

引用类型变量也可以在声明时完成初始化：

```text
引用类型 变量名 = 初始引用;
```

其中，左侧的“引用类型”通常是类名，变量名由程序员自定义，右侧应是一个对象引用。

对于普通类对象，常见写法是使用 `new` 创建对象，再将对象引用赋值给变量：

```text
类名 变量名 = new 类名(参数);
```

常见写法如下：

```java
String name = "ZhangSan";
StringBuilder builder = new StringBuilder("Java");
```

第一行中，`String` 是引用类型，`name` 是引用类型变量，`"ZhangSan"` 是字符串对象的引用。`String` 比较特殊，实际开发中通常直接使用字符串直接量赋值。

第二行中，`StringBuilder` 是引用类型，`builder` 是引用类型变量，`new StringBuilder("Java")` 会创建一个 `StringBuilder` 对象，并将该对象的引用赋值给 `builder`。

先声明、再赋值的写法如下：

```java
String title;
title = "Java";
```

局部引用类型变量在使用前也必须完成赋值。以下代码无法通过编译：

```java
String title;
System.out.println(title); // 错误：局部变量 title 尚未初始化
```

引用类型变量还可以赋值为 `null`，表示当前没有指向任何对象：

```java
String text = null;
```

### 6.2 基本类型变量与引用类型变量的差异

| 对比项 | 基本类型变量 | 引用类型变量 |
| --- | --- | --- |
| 保存内容 | 具体数据值 | 对象引用 |
| 典型类型 | `int`、`double`、`char`、`boolean` | `String`、`StringBuilder`、自定义类 |
| 是否可为 `null` | 不可以 | 可以 |
| 赋值特点 | 复制具体值 | 复制引用 |
| 使用 `==` 比较时 | 比较值是否相等 | 比较引用是否指向同一对象 |

示例：

```java
int a = 10;
int b = a;
b = 20;
System.out.println(a); // 10
System.out.println(b); // 20
```

基本类型变量赋值时复制的是值本身。修改 `b` 不会影响 `a`。

引用类型变量赋值时复制的是引用。

```java
StringBuilder first = new StringBuilder("Java");
StringBuilder second = first;

second.append(" Language");

System.out.println(first);  // Java Language
System.out.println(second); // Java Language
```

`first` 与 `second` 保存的是同一个对象的引用。通过 `second` 修改对象内容后，通过 `first` 也能看到修改结果。

### 6.3 `null`

`null` 表示引用类型变量当前没有指向任何对象。

```java
String name = null;
```

对 `null` 调用方法会导致运行时错误，即 `NullPointerException`。

```java
String name = null;
System.out.println(name.length()); // 运行时报错
```

基本类型变量不能赋值为 `null`。

```java
int age = null; // 错误
```

## 7. 运算符基础

### 7.1 算术运算符

常见算术运算符如下：

| 运算符 | 含义 | 示例 |
| --- | --- | --- |
| `+` | 加法 | `a + b` |
| `-` | 减法 | `a - b` |
| `*` | 乘法 | `a * b` |
| `/` | 除法 | `a / b` |
| `%` | 取余 | `a % b` |

示例：

```java
int a = 10;
int b = 3;

System.out.println(a + b); // 13
System.out.println(a - b); // 7
System.out.println(a * b); // 30
System.out.println(a / b); // 3
System.out.println(a % b); // 1
```

整数除法会舍弃小数部分。

```java
int result = 10 / 4;
System.out.println(result); // 2
```

只要参与除法运算的一方是浮点类型，结果就是浮点结果。

```java
double result = 10.0 / 4;
System.out.println(result); // 2.5
```

### 7.2 自增与自减运算符

`++` 表示变量自身加 1，`--` 表示变量自身减 1。

```java
int count = 5;
count++;
System.out.println(count); // 6

count--;
System.out.println(count); // 5
```

`++` 与 `--` 可以放在变量前，也可以放在变量后。

```java
int a = 5;
int b = ++a;
System.out.println(a); // 6
System.out.println(b); // 6

int x = 5;
int y = x++;
System.out.println(x); // 6
System.out.println(y); // 5
```

前置自增先改变变量值，再参与表达式运算；后置自增先使用变量当前值，再改变变量值。初学阶段应避免在复杂表达式中混用自增、自减，防止代码难以阅读。

### 7.3 比较运算符

比较运算符用于比较两个值，结果一定是 `boolean`。

| 运算符 | 含义 |
| --- | --- |
| `>` | 大于 |
| `<` | 小于 |
| `>=` | 大于等于 |
| `<=` | 小于等于 |
| `==` | 等于 |
| `!=` | 不等于 |

示例：

```java
int age = 18;

System.out.println(age >= 18); // true
System.out.println(age == 20); // false
System.out.println(age != 20); // true
```

对于基本类型变量，`==` 比较的是值是否相等。

```java
int a = 10;
int b = 10;
System.out.println(a == b); // true
```

对于引用类型变量，`==` 比较的是两个引用是否指向同一个对象，而不是对象内容是否相同。`String` 比较尤其需要注意，详见第 9 节。

### 7.4 逻辑运算符

逻辑运算符用于组合或处理 `boolean` 表达式。

| 运算符 | 含义 | 示例 |
| --- | --- | --- |
| `&&` | 逻辑与，两个条件都为 `true` 时结果为 `true` | `age >= 18 && score >= 60` |
| `\|\|` | 逻辑或，至少一个条件为 `true` 时结果为 `true` | `age < 18 \|\| score < 60` |
| `!` | 逻辑非，取反 | `!passed` |

示例：

```java
int age = 20;
double score = 85.5;

boolean canPass = age >= 18 && score >= 60;
System.out.println(canPass); // true
```

`&&` 与 `||` 具有短路特性：

1. 对于 `&&`，如果左侧已经是 `false`，右侧不会继续计算。
2. 对于 `||`，如果左侧已经是 `true`，右侧不会继续计算。

示例：

```java
int total = 0;

if (total != 0 && 100 / total > 10) {
    System.out.println("valid");
}
```

由于 `total != 0` 为 `false`，右侧 `100 / total > 10` 不会执行，因此避免了除以 0 的错误。

### 7.5 赋值运算符与复合赋值运算符

`=` 用于赋值。

```java
int age = 18;
age = 19;
```

复合赋值运算符将运算与赋值合并。

| 运算符 | 基本含义 |
| --- | --- |
| `+=` | 在原值基础上加右侧值后再赋值 |
| `-=` | 在原值基础上减右侧值后再赋值 |
| `*=` | 在原值基础上乘右侧值后再赋值 |
| `/=` | 在原值基础上除以右侧值后再赋值 |
| `%=` | 在原值基础上对右侧值取余后再赋值 |

示例：

```java
int score = 80;
score += 5;
System.out.println(score); // 85
```

## 8. 基本类型变量之间的隐式类型转换

### 8.1 隐式类型转换的含义

隐式类型转换是指 Java 编译器在不需要程序员显式写出强制转换的情况下，自动完成的类型转换。

常见场景包括：

1. 将取值范围较小的类型赋值给取值范围较大的类型。
2. 多种数值类型共同参与运算。
3. `byte`、`short`、`char` 参与算术运算时自动提升为 `int`。

隐式类型转换必须符合 Java 语言规定的转换方向。一般来说，从较窄的数值类型转换到较宽的数值类型，可以自动完成；反方向转换需要显式强制类型转换。需要注意，自动转换不完全等同于“没有任何精度损失”，例如 `long` 转换为 `float` 时可能丢失低位精度，但该转换仍然属于 Java 允许的隐式转换。

### 8.2 赋值场景中的自动类型转换

赋值时，Java 允许把较窄的数值类型赋值给较宽的数值类型。该规则称为拓宽基本类型转换。

```text
byte -> short -> int -> long -> float -> double
char -> int -> long -> float -> double
```

示例：

```java
byte b = 10;
short s = b;
int i = s;
long l = i;
float f = l;
double d = f;
```

需要注意，`char` 可以自动转换为 `int`、`long`、`float`、`double`，但不能自动转换为 `short`。`short` 也不能自动转换为 `char`。虽然 `char` 与 `short` 都占 2 字节，但二者取值范围不同：`char` 表示 `0` 到 `65535` 的无符号字符编码，`short` 表示 `-32768` 到 `32767` 的有符号整数。

```java
char c = 'A';
int value = c;     // 正确
// short s = c;    // 错误

short number = 65;
// char ch = number; // 错误
```

`boolean` 不参与任何数值类型转换。

```java
boolean flag = true;
// int value = flag; // 错误
```

### 8.3 整数常量赋值的特殊规则

整数直接量默认是 `int` 类型，但在给 `byte`、`short`、`char` 赋值时，如果该整数常量没有超出目标类型范围，编译器允许直接赋值。

```java
byte b = 10;
short s = 100;
char c = 65;
```

以下代码无法通过编译，因为数值超出了 `byte` 的取值范围：

```java
byte b = 128; // 错误：byte 最大值为 127
```

该规则只适用于编译期可以确定的整数常量，不适用于普通变量参与的表达式。

```java
byte a = 10;
byte b = 20;
// byte sum = a + b; // 错误：a + b 的结果类型是 int
```

### 8.4 表达式运算中的自动类型提升

多个数值类型共同参与算术运算时，Java 会先对操作数进行类型提升，再执行运算。二元数值运算通常遵循以下规则：

1. 如果任意一方是 `double`，另一方会提升为 `double`，结果也是 `double`。
2. 否则，如果任意一方是 `float`，另一方会提升为 `float`，结果也是 `float`。
3. 否则，如果任意一方是 `long`，另一方会提升为 `long`，结果也是 `long`。
4. 否则，参与运算的 `byte`、`short`、`char` 都会先提升为 `int`，结果也是 `int`。

示例：

```java
int a = 10;
double b = 3.5;
double result = a + b;
System.out.println(result); // 13.5
```

`a` 在参与运算时自动转换为 `double`，因此 `a + b` 的结果是 `double`。

再看一个包含 `long` 的示例：

```java
int a = 10;
long b = 20L;
long result = a + b;
System.out.println(result); // 30
```

`a` 会先提升为 `long`，因此 `a + b` 的结果是 `long`。

需要注意：

1. `boolean` 不参与数值类型转换。
2. `byte`、`short`、`char` 在算术运算中会先提升为 `int`。
3. 表达式的结果类型通常由参与运算的最高级类型决定。

### 8.5 `byte`、`short`、`char` 的运算提升

`byte`、`short` 和 `char` 只要参与算术运算，结果通常就是 `int`。

```java
byte a = 10;
byte b = 20;
int result = a + b;
```

以下写法无法通过编译：

```java
byte a = 10;
byte b = 20;
byte result = a + b; // 错误：a + b 的结果是 int
```

`short` 也遵循相同规则：

```java
short x = 100;
short y = 200;
int result = x + y;
```

`char` 参与运算时，会转换为对应的 Unicode 编码值。

```java
char c = 'A';
int value = c + 1;
System.out.println(value); // 66
```

如果希望得到下一个字符，可以使用强制类型转换：

```java
char c = 'A';
char next = (char) (c + 1);
System.out.println(next); // B
```

### 8.6 整数与浮点数共同运算

当整数类型与浮点类型共同参与运算时，结果会提升为浮点类型。

```java
int count = 5;
double price = 2.5;
double total = count * price;
System.out.println(total); // 12.5
```

如果表达式中出现 `double`，结果通常为 `double`。

```java
int a = 10;
long b = 20L;
double c = 3.5;

double result = a + b + c;
System.out.println(result); // 33.5
```

### 8.7 强制类型转换

当取值范围较大的类型赋值给取值范围较小的类型时，Java 不会自动完成转换，通常需要使用强制类型转换。

```text
目标类型 变量名 = (目标类型) 原始数据;
```

```java
double price = 19.8;
int value = (int) price;
System.out.println(value); // 19
```

浮点数强制转换为整数时，只保留整数部分，不会四舍五入。

```java
double d = 9.99;
int n = (int) d;
System.out.println(n); // 9
```

强制类型转换可能造成精度丢失或数值溢出。

```java
int value = 130;
byte b = (byte) value;
System.out.println(b); // -126
```

`byte` 的取值范围是 `-128` 到 `127`。将 `130` 强制转换为 `byte` 时，结果已经不是原来的数值。这类转换在实际代码中必须谨慎使用。

### 8.8 复合赋值运算符中的隐含转换

复合赋值运算符不仅包含运算和赋值，还包含一次由语法规则隐含完成的类型转换。

例如：

```java
byte b = 10;
b += 1;
System.out.println(b); // 11
```

上述代码可以通过编译。原因是 `b += 1` 大致等价于：

```java
b = (byte) (b + 1);
```

但普通赋值写法不能省略强制类型转换：

```java
byte b = 10;
// b = b + 1; // 错误：b + 1 的结果类型是 int
```

原因如下：

1. `b` 是 `byte` 类型。
2. `1` 是 `int` 类型整数常量。
3. `b + 1` 运算时，`b` 会先提升为 `int`。
4. `b + 1` 的结果类型是 `int`。
5. `int` 结果不能直接赋值给 `byte` 变量。

复合赋值中的隐含转换不代表一定安全。如果运算结果超出左侧变量类型的取值范围，仍然可能发生溢出。

```java
byte b = 127;
b += 1;
System.out.println(b); // -128
```

因此，复合赋值运算符虽然写法简洁，但仍然需要按照基本类型转换规则理解其结果。

## 9. `String` 变量与字符串常量池

### 9.1 `String` 的基本特点

`String` 是引用类型，不是基本类型。它用于表示字符串，即一段文本内容。

```java
String name = "ZhangSan";
String message = "Hello Java";
```

字符串直接量必须使用双引号。

```java
String s = "A";
```

### 9.2 字符串常量池

字符串常量池用于保存通过字符串直接量形式出现的 `String` 对象。字符串直接量指不使用 `new`，直接写在代码中的字符串，例如 `"Java"`、`"Hello"`。

示例：

```java
String a = "Java";
String b = "Java";
```

上面代码没有使用 `new` 创建字符串对象。Java 会在字符串常量池中保存内容为 `"Java"` 的字符串对象，并让 `a` 和 `b` 复用这个对象。

```java
System.out.println(a == b); // true
```

字符串常量池的主要作用是复用相同内容的字符串，减少重复字符串对象的创建。

### 9.3 `new String` 与字符串常量池

使用 `new String(...)` 会创建新的字符串对象。这个由 `new` 创建出来的对象不在字符串常量池中。

```java
String a = "Java";
String b = new String("Java");
```

其中：

1. `a` 指向字符串常量池中的 `"Java"` 对象。
2. `b` 指向 `new String("Java")` 创建出来的新对象。
3. 两个对象的内容相同，但不是同一个对象。

### 9.4 `==` 与 `equals`

`==` 用于比较两个引用是否指向同一个对象。它不会直接比较字符串内容。

因此，常量池中的字符串对象和 `new` 创建出来的字符串对象使用 `==` 比较时，结果为 `false`。

```java
String a = "Java";
String b = new String("Java");

System.out.println(a == b);      // false
System.out.println(a.equals(b)); // true
```

`a == b` 的结果为 `false`，因为 `a` 和 `b` 保存的引用不同。

`a.equals(b)` 的结果为 `true`，因为 `equals` 比较的是字符串内容。

因此，判断字符串内容是否相同，应使用 `equals` 方法，而不应直接使用 `==`。

如果变量可能为 `null`，可以将确定不为 `null` 的字符串常量写在前面调用 `equals`。

```java
String input = null;

System.out.println("admin".equals(input)); // false，不会报错
// System.out.println(input.equals("admin")); // 运行时报错
```

结论：

1. 基本类型使用 `==` 比较值。
2. 引用类型使用 `==` 比较是否为同一个对象。
3. `String` 比较内容应使用 `equals`。
4. 不应因为部分字符串直接量使用 `==` 返回 `true`，就把 `==` 当作字符串内容比较方式。

## 10. 基本类型变量与 `String` 变量之间的运算

### 10.1 `+` 的双重含义

`+` 在 Java 中有两种常见含义：

1. 当两侧都是数值类型时，表示加法运算。
2. 当任意一侧是 `String` 时，表示字符串拼接。

示例：

```java
int a = 10;
int b = 20;

System.out.println(a + b); // 30
```

```java
String text = "Result: ";
int value = 30;

System.out.println(text + value); // Result: 30
```

### 10.2 `String` 与其他基本类型拼接

`String` 可以与 `int`、`double`、`char`、`boolean` 等类型使用 `+` 进行拼接。

```java
String name = "LiMing";
int age = 18;
double score = 92.5;
char grade = 'A';
boolean passed = true;

System.out.println("姓名：" + name);
System.out.println("年龄：" + age);
System.out.println("成绩：" + score);
System.out.println("等级：" + grade);
System.out.println("是否通过：" + passed);
```

拼接时，非字符串数据会转换为对应的字符串表现形式。

### 10.3 字符串拼接中的计算顺序

表达式从左到右计算。只要运算过程中出现 `String`，后续的 `+` 通常会按字符串拼接处理。

如果字符串出现在表达式后半部分，字符串前面的数值运算仍然会先按数值加法处理。

```java
int a = 10;
int b = 20;

System.out.println(a + b + "Hello World!");
```

输出结果：

```text
30Hello World!
```

原因如下：

1. `a + b` 先执行，两侧都是 `int`，结果是数值 `30`。
2. `30 + "Hello World!"` 再执行，此时右侧是 `String`，因此执行字符串拼接。
3. 最终结果为 `"30Hello World!"`。

如果字符串出现在表达式开头，后续的 `+` 会从左到右依次执行字符串拼接。

```java
int a = 10;
int b = 20;

System.out.println("result = " + a + b);
```

输出结果：

```text
result = 1020
```

原因如下：

1. `"result = " + a` 先执行，结果是字符串 `"result = 10"`。
2. `"result = 10" + b` 继续执行字符串拼接。
3. 最终结果为 `"result = 1020"`。

如果希望先进行数值加法，应使用小括号改变计算顺序。

```java
System.out.println("result = " + (a + b));
```

输出结果：

```text
result = 30
```

### 10.4 `char` 与 `String` 运算的区别

`char` 与数值类型相加时，会先转换为整数编码值。

```java
char c = 'A';
System.out.println(c + 1); // 66
```

`char` 与 `String` 相加时，执行字符串拼接。

```java
char c = 'A';
System.out.println(c + "1"); // A1
```

如果需要输出字符后接数字，应确保表达式中存在字符串。

```java
char c = 'A';
int number = 1;

System.out.println("" + c + number); // A1
```

## 11. 代码示例

在 IDEA 中创建 `VariableDemo.java`，输入以下代码并运行。

```java
public class VariableDemo {
    public static void main(String[] args) {
        int age = 18;
        double score = 92.5;
        char grade = 'A';
        boolean passed = true;
        String name = "LiMing";

        System.out.println("姓名：" + name);
        System.out.println("年龄：" + age);
        System.out.println("成绩：" + score);
        System.out.println("等级：" + grade);
        System.out.println("是否通过：" + passed);

        int a = 10;
        int b = 20;
        System.out.println("a + b = " + (a + b));
        System.out.println("未加括号：" + a + b);

        byte x = 10;
        byte y = 20;
        int sum = x + y;
        System.out.println("byte 运算结果：" + sum);

        char c = 'A';
        System.out.println("字符参与数值运算：" + (c + 1));
        System.out.println("字符参与字符串拼接：" + c + "1");

        String s1 = "Java";
        String s2 = "Java";
        String s3 = new String("Java");

        System.out.println("s1 == s2：" + (s1 == s2));
        System.out.println("s1 == s3：" + (s1 == s3));
        System.out.println("s1.equals(s3)：" + s1.equals(s3));
    }
}
```

运行后应重点观察以下输出差异：

1. `"a + b = " + (a + b)` 与 `"未加括号：" + a + b` 的结果差异。
2. `byte` 类型变量相加后结果类型为 `int`。
3. `char` 与数字相加、`char` 与字符串拼接的结果差异。
4. `s1 == s2`、`s1 == s3`、`s1.equals(s3)` 的结果差异。

## 12. 练习

### 12.1 基础练习

创建 `VariablePractice.java`，完成以下要求：

1. 声明并初始化一个 `String` 类型变量表示姓名。
2. 声明并初始化一个 `int` 类型变量表示年龄。
3. 声明并初始化一个 `double` 类型变量表示身高。
4. 声明并初始化一个 `char` 类型变量表示等级。
5. 声明并初始化一个 `boolean` 类型变量表示是否通过。
6. 使用字符串拼接输出上述所有变量。
7. 至少写出一个使用小括号改变字符串拼接顺序的表达式。

### 12.2 类型转换练习

在同一个文件中继续完成以下代码验证：

1. 使用两个 `byte` 变量相加，并用 `int` 接收结果。
2. 使用一个 `int` 变量和一个 `double` 变量相加，并用 `double` 接收结果。
3. 使用一个 `char` 变量与整数相加，并输出结果。
4. 将一个 `double` 变量强制转换为 `int`，观察小数部分的处理方式。

### 12.3 字符串比较练习

在同一个文件中继续完成以下代码验证：

1. 声明 `String a = "Java";`。
2. 声明 `String b = "Java";`。
3. 声明 `String c = new String("Java");`。
4. 分别输出 `a == b`、`a == c`、`a.equals(c)` 的结果。
5. 用文字说明为什么不能直接使用 `==` 判断字符串内容是否相同。

## 13. 验收标准

满足以下条件视为完成 Task 1：

1. 能准确说明 Java 基本类型变量与引用类型变量的区别。
2. 能正确写出 8 种基本类型，并说明 `int`、`double`、`char`、`boolean` 的典型用法。
3. 能解释 `String` 是引用类型，而不是基本类型。
4. 能说明字符串常量池的基本作用。
5. 能说明为什么字符串内容比较应使用 `equals`，不能依赖 `==`。
6. 能正确判断 `+` 在数值加法和字符串拼接中的不同含义。
7. 能解释 `"result = " + a + b` 与 `"result = " + (a + b)` 的结果差异。
8. 能说明 `byte`、`short`、`char` 参与算术运算时会提升为 `int`。
9. 能说明整数与浮点数共同运算时的结果类型变化。
10. 能独立完成 `VariablePractice.java` 并运行得到正确结果。

## 14. 提交规范

1. 学习/实验总结报告统一命名为 `Task_1_report.md`，放置在 `Task_1/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_1/src/` 目录下（或在 `Task_1/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_1/` 目录中的本阶段内容。
