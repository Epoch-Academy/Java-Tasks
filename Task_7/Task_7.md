# Task 7：Java 反射与网络编程

## 1. 任务说明

本任务引入 Java 反射机制和网络编程基础，重点包括 `Class` 对象、构造方法反射、字段反射、方法反射、注解反射、动态代理、网络基础概念、现代网络模型、常见协议、请求与响应、HTTP 请求方法，以及 Java 中常见的网络编程方式。

反射是一种在程序运行期间观察和操作类信息的能力。前面的任务中，我们通常在写代码时就明确知道要创建哪个类、调用哪个方法、访问哪个属性。反射则允许程序在运行时根据类名、方法名、字段名等信息动态完成这些操作。

网络编程用于让不同设备、不同程序之间进行数据交换。现代软件中，浏览器访问网页、手机 App 获取数据、后端服务之间互相调用、多人在线系统通信，都离不开网络编程。

本任务的学习顺序为：

1. 先学习反射。
2. 再学习网络中的常见概念。
3. 最后学习如何在 Java 中进行网络编程。

## 2. 知识范围

本任务包含以下内容：

1. 反射的含义。
2. `Class` 对象。
3. 获取 `Class` 对象的方式。
4. 通过反射获取类的基本信息。
5. 通过反射操作构造方法。
6. 通过反射操作成员变量。
7. 通过反射操作成员方法。
8. `setAccessible`。
9. 注解基础。
10. 通过反射读取注解。
11. 动态代理基础。
12. 反射的使用场景和注意事项。
13. 网络的含义。
14. IP 地址、端口、域名和 DNS。
15. 现代网络模型。
16. 常见网络协议。
17. 请求和响应。
18. HTTP 请求方法。
19. URL、URI、查询参数和请求头。
20. 状态码。
21. Java 中的 `InetAddress`。
22. Java 中的 `URI` 和 `URL`。
23. Java `HttpClient`。
24. TCP 编程。
25. UDP 编程。
26. 网络编程中的异常和资源关闭。

## 3. 反射基础

### 3.1 什么是反射

反射是 Java 提供的一种运行时能力。

普通代码中，创建对象和调用方法通常是固定写好的。

```java
Student student = new Student("Alice", 18);
student.introduce();
```

反射可以在运行时根据类信息完成类似操作。

```java
Class<?> clazz = Class.forName("Student");
Object object = clazz.getConstructor(String.class, int.class).newInstance("Alice", 18);
clazz.getMethod("introduce").invoke(object);
```

反射可以让程序在运行时做这些事情：

1. 获取类的名称、包名、父类和接口。
2. 获取类中的构造方法。
3. 获取类中的成员变量。
4. 获取类中的成员方法。
5. 创建对象。
6. 读取或修改对象字段。
7. 调用对象方法。
8. 读取注解信息。

### 3.2 为什么需要反射

反射常用于框架和工具代码中。

例如：

1. 测试框架根据注解查找测试方法。
2. Web 框架根据请求路径调用对应方法。
3. JSON 工具根据字段名给对象赋值。
4. ORM 框架根据类和字段映射数据库表。
5. 配置文件中写类名，程序运行时创建对应对象。

普通业务代码中不一定频繁使用反射，但理解反射有助于理解很多框架的底层思想。

### 3.3 反射和普通调用的区别

| 对比项 | 普通调用 | 反射 |
| ------ | -------- | ---- |
| 类和方法 | 编写代码时明确 | 运行时动态确定 |
| 编译检查 | 强 | 较弱 |
| 代码可读性 | 通常更清晰 | 通常更复杂 |
| 性能 | 通常更好 | 通常有额外开销 |
| 使用场景 | 普通业务逻辑 | 框架、工具、通用代码 |

基础阶段应记住：能用普通方式清楚完成的业务逻辑，不要为了“高级”而使用反射。

## 4. `Class` 对象

### 4.1 什么是 `Class` 对象

每个 Java 类在运行时都会有一个对应的 `Class` 对象。

`Class` 对象保存了类的结构信息，例如：

1. 类名。
2. 包名。
3. 父类。
4. 接口。
5. 构造方法。
6. 成员变量。
7. 成员方法。
8. 注解。

可以把 `Class` 对象理解为“类在运行时的说明书”。

示例类：

```java
public class Student {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public void introduce() {
        System.out.println(name + "-" + age);
    }
}
```

通过 `Student` 的 `Class` 对象，可以查看和操作这个类的结构。

### 4.2 获取 `Class` 对象的三种方式

常见方式有三种。

第一种：类名 `.class`。

```java
Class<Student> clazz = Student.class;
```

第二种：对象 `.getClass()`。

```java
Student student = new Student();
Class<?> clazz = student.getClass();
```

第三种：`Class.forName`。

```java
Class<?> clazz = Class.forName("Student");
```

如果类有包名，需要写完整类名。

```java
Class<?> clazz = Class.forName("com.example.Student");
```

### 4.3 三种方式的区别

| 写法 | 常见用途 |
| ---- | -------- |
| `Student.class` | 编写代码时已经知道类 |
| `student.getClass()` | 已经有对象，需要获取它的运行时类型 |
| `Class.forName("完整类名")` | 根据字符串类名动态加载类 |

示例：

```java
public class ClassObjectDemo {
    public static void main(String[] args) throws ClassNotFoundException {
        Class<Student> c1 = Student.class;

        Student student = new Student();
        Class<?> c2 = student.getClass();

        Class<?> c3 = Class.forName("Student");

        System.out.println(c1 == c2);
        System.out.println(c2 == c3);
    }
}
```

运行后应观察：同一个类在运行时通常只有一个对应的 `Class` 对象。

### 4.4 获取类的基本信息

```java
public class ClassInfoDemo {
    public static void main(String[] args) {
        Class<Student> clazz = Student.class;

        System.out.println(clazz.getName());
        System.out.println(clazz.getSimpleName());
        System.out.println(clazz.getPackageName());
        System.out.println(clazz.getSuperclass().getName());
    }
}
```

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `getName()` | 获取完整类名 |
| `getSimpleName()` | 获取简单类名 |
| `getPackageName()` | 获取包名 |
| `getSuperclass()` | 获取父类 |
| `getInterfaces()` | 获取实现的接口 |
| `getModifiers()` | 获取修饰符 |

## 5. 构造方法反射

### 5.1 `Constructor`

构造方法在反射中使用 `Constructor` 表示。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `getConstructors()` | 获取所有 `public` 构造方法 |
| `getDeclaredConstructors()` | 获取所有声明的构造方法 |
| `getConstructor(...)` | 获取指定 `public` 构造方法 |
| `getDeclaredConstructor(...)` | 获取指定声明的构造方法 |
| `newInstance(...)` | 调用构造方法创建对象 |

示例：

```java
import java.lang.reflect.Constructor;

public class ConstructorInfoDemo {
    public static void main(String[] args) {
        Class<Student> clazz = Student.class;

        Constructor<?>[] constructors = clazz.getDeclaredConstructors();

        for (Constructor<?> constructor : constructors) {
            System.out.println(constructor);
        }
    }
}
```

### 5.2 通过无参构造创建对象

```java
import java.lang.reflect.Constructor;

public class NoArgsConstructorDemo {
    public static void main(String[] args) throws Exception {
        Class<Student> clazz = Student.class;

        Constructor<Student> constructor = clazz.getConstructor();
        Student student = constructor.newInstance();

        student.introduce();
    }
}
```

其中：

1. `getConstructor()` 获取 `public` 无参构造方法。
2. `newInstance()` 调用构造方法创建对象。

### 5.3 通过有参构造创建对象

```java
import java.lang.reflect.Constructor;

public class ArgsConstructorDemo {
    public static void main(String[] args) throws Exception {
        Class<Student> clazz = Student.class;

        Constructor<Student> constructor = clazz.getConstructor(String.class, int.class);
        Student student = constructor.newInstance("Alice", 18);

        student.introduce();
    }
}
```

`getConstructor(String.class, int.class)` 表示获取参数列表为 `(String, int)` 的构造方法。

### 5.4 操作非公开构造方法

如果构造方法是 `private`，可以使用 `getDeclaredConstructor` 获取。

```java
public class Secret {
    private String value;

    private Secret(String value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return value;
    }
}
```

反射创建对象：

```java
import java.lang.reflect.Constructor;

public class PrivateConstructorDemo {
    public static void main(String[] args) throws Exception {
        Class<Secret> clazz = Secret.class;

        Constructor<Secret> constructor = clazz.getDeclaredConstructor(String.class);
        constructor.setAccessible(true);

        Secret secret = constructor.newInstance("hidden");

        System.out.println(secret);
    }
}
```

`setAccessible(true)` 表示允许反射绕过 Java 语言层面的访问检查。

基础阶段应理解它的作用，但不要滥用它破坏封装。

## 6. 字段反射

### 6.1 `Field`

成员变量在反射中使用 `Field` 表示。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `getFields()` | 获取所有 `public` 字段 |
| `getDeclaredFields()` | 获取所有声明字段 |
| `getField(String name)` | 获取指定 `public` 字段 |
| `getDeclaredField(String name)` | 获取指定声明字段 |
| `get(Object obj)` | 获取对象字段值 |
| `set(Object obj, Object value)` | 设置对象字段值 |

示例：

```java
import java.lang.reflect.Field;

public class FieldInfoDemo {
    public static void main(String[] args) {
        Class<Student> clazz = Student.class;

        Field[] fields = clazz.getDeclaredFields();

        for (Field field : fields) {
            System.out.println(field.getName());
            System.out.println(field.getType().getSimpleName());
        }
    }
}
```

### 6.2 读取字段值

```java
import java.lang.reflect.Field;

public class FieldGetDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 18);

        Field nameField = Student.class.getDeclaredField("name");
        nameField.setAccessible(true);

        Object value = nameField.get(student);

        System.out.println(value);
    }
}
```

其中：

1. `getDeclaredField("name")` 获取名为 `name` 的字段。
2. `setAccessible(true)` 允许访问 `private` 字段。
3. `get(student)` 获取 `student` 对象中该字段的值。

### 6.3 修改字段值

```java
import java.lang.reflect.Field;

public class FieldSetDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 18);

        Field ageField = Student.class.getDeclaredField("age");
        ageField.setAccessible(true);

        ageField.set(student, 20);

        student.introduce();
    }
}
```

`ageField.set(student, 20)` 表示把 `student` 对象中的 `age` 字段改为 `20`。

### 6.4 获取字段修饰符

字段修饰符可以通过 `getModifiers()` 获取。

```java
import java.lang.reflect.Field;
import java.lang.reflect.Modifier;

public class FieldModifierDemo {
    public static void main(String[] args) {
        Field[] fields = Student.class.getDeclaredFields();

        for (Field field : fields) {
            int modifiers = field.getModifiers();

            System.out.println(Modifier.toString(modifiers));
            System.out.println(field.getName());
        }
    }
}
```

`Modifier.toString(modifiers)` 可以把修饰符数字转换成可读字符串。

## 7. 方法反射

### 7.1 `Method`

成员方法在反射中使用 `Method` 表示。

常见方法：

| 方法 | 含义 |
| ---- | ---- |
| `getMethods()` | 获取所有 `public` 方法，包括继承的方法 |
| `getDeclaredMethods()` | 获取当前类声明的所有方法 |
| `getMethod(String name, Class<?>... parameterTypes)` | 获取指定 `public` 方法 |
| `getDeclaredMethod(String name, Class<?>... parameterTypes)` | 获取指定声明方法 |
| `invoke(Object obj, Object... args)` | 调用方法 |

示例：

```java
import java.lang.reflect.Method;

public class MethodInfoDemo {
    public static void main(String[] args) {
        Method[] methods = Student.class.getDeclaredMethods();

        for (Method method : methods) {
            System.out.println(method.getName());
            System.out.println(method.getReturnType().getSimpleName());
        }
    }
}
```

### 7.2 调用无参方法

```java
import java.lang.reflect.Method;

public class MethodInvokeDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 18);

        Method method = Student.class.getMethod("introduce");
        method.invoke(student);
    }
}
```

其中：

1. `getMethod("introduce")` 获取 `public` 无参方法。
2. `invoke(student)` 表示在 `student` 对象上调用该方法。

### 7.3 调用有参方法

示例类：

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

反射调用：

```java
import java.lang.reflect.Method;

public class MethodArgsDemo {
    public static void main(String[] args) throws Exception {
        Calculator calculator = new Calculator();

        Method method = Calculator.class.getMethod("add", int.class, int.class);
        Object result = method.invoke(calculator, 10, 20);

        System.out.println(result);
    }
}
```

`method.invoke` 的返回值类型是 `Object`。如果原方法返回基本类型，反射调用结果会自动装箱。

### 7.4 调用静态方法

示例类：

```java
public class TextUtils {
    public static boolean isBlank(String text) {
        return text == null || text.length() == 0;
    }
}
```

反射调用静态方法：

```java
import java.lang.reflect.Method;

public class StaticMethodDemo {
    public static void main(String[] args) throws Exception {
        Method method = TextUtils.class.getMethod("isBlank", String.class);

        Object result = method.invoke(null, "");

        System.out.println(result);
    }
}
```

调用静态方法时，`invoke` 的第一个参数可以传入 `null`。

## 8. `setAccessible`

### 8.1 `setAccessible` 的含义

`setAccessible(true)` 用于允许反射访问非公开成员。

它常用于：

1. 访问 `private` 构造方法。
2. 访问 `private` 成员变量。
3. 调用 `private` 成员方法。

示例：

```java
import java.lang.reflect.Method;

public class PrivateMethodDemo {
    public static void main(String[] args) throws Exception {
        Student student = new Student("Alice", 18);

        Method method = Student.class.getDeclaredMethod("secret");
        method.setAccessible(true);

        method.invoke(student);
    }
}
```

如果 `secret` 是 `private` 方法，需要使用 `getDeclaredMethod` 和 `setAccessible(true)`。

### 8.2 使用注意事项

`setAccessible(true)` 很强大，但也有风险。

注意事项：

1. 它会破坏封装。
2. 它可能让代码更难维护。
3. 它可能受到模块系统或安全策略限制。
4. 它通常适合框架、测试工具和底层工具代码。

基础阶段应能看懂和简单使用，但不要在普通业务代码中过度依赖。

## 9. 注解与反射

### 9.1 什么是注解

注解是一种写在类、方法、字段等位置上的标记信息。

常见注解：

1. `@Override`：表示方法重写。
2. `@Deprecated`：表示不推荐继续使用。
3. `@SuppressWarnings`：抑制编译器警告。

示例：

```java
public class AnnotationIntroDemo {
    @Override
    public String toString() {
        return "demo";
    }
}
```

注解本身不会自动改变代码逻辑。很多注解需要编译器、框架或反射代码读取后才产生效果。

### 9.2 自定义注解

自定义注解使用 `@interface`。

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Table {
    String value();
}
```

其中：

1. `@interface` 表示定义注解。
2. `@Retention(RetentionPolicy.RUNTIME)` 表示注解在运行时仍然保留，可以被反射读取。
3. `@Target(ElementType.TYPE)` 表示该注解可以写在类、接口等类型上。
4. `String value();` 表示注解中有一个名为 `value` 的属性。

使用注解：

```java
@Table("students")
public class StudentEntity {
    private String name;
    private int age;
}
```

### 9.3 读取类上的注解

```java
public class AnnotationReadDemo {
    public static void main(String[] args) {
        Class<StudentEntity> clazz = StudentEntity.class;

        if (clazz.isAnnotationPresent(Table.class)) {
            Table table = clazz.getAnnotation(Table.class);
            System.out.println(table.value());
        }
    }
}
```

其中：

1. `isAnnotationPresent` 判断是否存在某个注解。
2. `getAnnotation` 获取指定注解对象。
3. `table.value()` 读取注解属性。

### 9.4 字段注解

定义字段注解：

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.FIELD)
public @interface Column {
    String value();
}
```

使用字段注解：

```java
@Table("students")
public class StudentEntity {
    @Column("student_name")
    private String name;

    @Column("student_age")
    private int age;
}
```

读取字段注解：

```java
import java.lang.reflect.Field;

public class FieldAnnotationDemo {
    public static void main(String[] args) {
        Field[] fields = StudentEntity.class.getDeclaredFields();

        for (Field field : fields) {
            Column column = field.getAnnotation(Column.class);

            if (column != null) {
                System.out.println(field.getName() + " -> " + column.value());
            }
        }
    }
}
```

## 10. 动态代理基础

### 10.1 什么是代理

代理可以理解为“由一个对象代替另一个对象完成方法调用，并在调用前后添加额外逻辑”。

示例：

```text
调用者 -> 代理对象 -> 真实对象
```

代理常用于：

1. 日志记录。
2. 权限检查。
3. 性能统计。
4. 事务管理。
5. 远程调用。

### 10.2 静态代理

先定义接口：

```java
public interface UserService {
    void register(String username);
}
```

真实对象：

```java
public class UserServiceImpl implements UserService {
    @Override
    public void register(String username) {
        System.out.println("注册用户：" + username);
    }
}
```

代理对象：

```java
public class UserServiceProxy implements UserService {
    private UserService target;

    public UserServiceProxy(UserService target) {
        this.target = target;
    }

    @Override
    public void register(String username) {
        System.out.println("开始记录日志");
        target.register(username);
        System.out.println("结束记录日志");
    }
}
```

使用：

```java
public class StaticProxyDemo {
    public static void main(String[] args) {
        UserService target = new UserServiceImpl();
        UserService proxy = new UserServiceProxy(target);

        proxy.register("Alice");
    }
}
```

静态代理需要手动写代理类。如果接口很多，代码会变多。

### 10.3 JDK 动态代理

JDK 动态代理可以在运行时创建代理对象。

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

public class DynamicProxyDemo {
    public static void main(String[] args) {
        UserService target = new UserServiceImpl();

        UserService proxy = (UserService) Proxy.newProxyInstance(
                target.getClass().getClassLoader(),
                target.getClass().getInterfaces(),
                new InvocationHandler() {
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        System.out.println("调用前：" + method.getName());
                        Object result = method.invoke(target, args);
                        System.out.println("调用后：" + method.getName());
                        return result;
                    }
                }
        );

        proxy.register("Alice");
    }
}
```

其中：

1. `Proxy.newProxyInstance` 创建代理对象。
2. 第一个参数是类加载器。
3. 第二个参数是代理对象要实现的接口。
4. 第三个参数是方法调用处理器。
5. `method.invoke(target, args)` 表示调用真实对象的方法。

JDK 动态代理要求被代理对象通常要实现接口。基础阶段只需要理解它的基本结构和用途。

## 11. 反射使用场景和注意事项

### 11.1 常见使用场景

反射常见使用场景：

1. 根据配置文件中的类名创建对象。
2. 自动给对象字段赋值。
3. 读取注解并执行对应逻辑。
4. 编写通用对象打印工具。
5. 编写测试框架。
6. 实现动态代理。
7. 框架底层扫描类和方法。

### 11.2 反射的优点

优点：

1. 灵活。
2. 可以在运行时动态操作类。
3. 可以编写更通用的工具代码。
4. 是理解框架底层的重要基础。

### 11.3 反射的缺点

缺点：

1. 代码更复杂。
2. 编译期检查变弱。
3. 可能影响性能。
4. 可能破坏封装。
5. 错误往往在运行时才暴露。

### 11.4 学习建议

基础阶段应达到以下目标：

1. 能获取 `Class` 对象。
2. 能读取类、字段、方法、构造方法信息。
3. 能通过反射创建对象、设置字段、调用方法。
4. 能理解注解和反射之间的关系。
5. 能看懂动态代理的基本结构。

不需要在普通业务代码中频繁使用反射。

## 12. 网络基础

### 12.1 什么是网络

网络是把多台设备连接起来，让它们可以交换数据的系统。

设备可以是：

1. 电脑。
2. 手机。
3. 服务器。
4. 路由器。
5. 平板。
6. 智能设备。

网络编程就是编写程序，让程序可以通过网络发送和接收数据。

示例：

```text
浏览器 -> 请求网页 -> 服务器
浏览器 <- 返回网页 <- 服务器
```

### 12.2 客户端和服务器

网络程序中经常有客户端和服务器两个角色。

客户端通常主动发起请求。

服务器通常等待请求并返回响应。

```text
客户端发起请求 -> 服务器处理请求 -> 服务器返回响应 -> 客户端接收结果
```

示例：

1. 浏览器是客户端，网站后端是服务器。
2. 手机 App 是客户端，后端接口服务是服务器。
3. 游戏客户端连接游戏服务器。

### 12.3 局域网和互联网

局域网是范围较小的网络，例如家庭网络、学校机房网络、公司内网。

互联网是全球范围的大型网络，由大量网络连接而成。

程序进行网络通信时，可以在同一台机器、本地局域网或互联网上通信。

### 12.4 网络通信需要解决的问题

两个程序要通过网络通信，至少需要解决这些问题：

1. 找到对方在哪台设备上。
2. 找到对方设备上的哪个程序。
3. 确定使用什么通信规则。
4. 确定如何组织发送的数据。
5. 确定如何处理返回的数据。

这些问题分别对应 IP 地址、端口、协议、请求格式和响应格式。

## 13. IP、端口、域名和 DNS

### 13.1 IP 地址

IP 地址用于标识网络中的一台设备。

常见 IPv4 地址示例：

```text
192.168.1.10
127.0.0.1
8.8.8.8
```

其中 `127.0.0.1` 表示本机地址，也叫回环地址。

常见 IPv6 地址示例：

```text
::1
2001:db8::1
```

基础阶段重点掌握 IPv4 的基本形式即可。

### 13.2 端口

一台设备上可能同时运行很多网络程序。端口用于区分同一台设备上的不同程序。

示例：

```text
192.168.1.10:8080
```

其中：

1. `192.168.1.10` 表示设备。
2. `8080` 表示该设备上的某个程序端口。

常见端口：

| 端口 | 常见用途 |
| ---- | -------- |
| `80` | HTTP |
| `443` | HTTPS |
| `22` | SSH |
| `3306` | MySQL |
| `6379` | Redis |
| `8080` | 常见开发服务端口 |

### 13.3 域名

IP 地址不便于记忆，因此可以使用域名。

示例：

```text
www.example.com
www.baidu.com
github.com
```

域名更适合人类记忆。程序真正通信时，仍然需要找到对应的 IP 地址。

### 13.4 DNS

DNS 用于把域名解析为 IP 地址。

```text
www.example.com -> 93.184.216.34
```

可以简单理解为：DNS 是互联网中的“地址查询系统”。

当浏览器访问网站时，大致过程是：

1. 输入域名。
2. 通过 DNS 查询域名对应的 IP。
3. 连接目标 IP 和端口。
4. 发送请求。
5. 接收响应。

## 14. 现代网络模型

### 14.1 为什么需要网络模型

网络通信很复杂。为了降低理解和实现难度，人们把网络通信分成多个层次。

每一层负责不同问题。

例如：

1. 如何表示电信号或无线信号。
2. 如何在局域网中传输数据。
3. 如何找到目标设备。
4. 如何保证数据可靠传输。
5. 如何表达网页请求、接口请求等应用数据。

### 14.2 OSI 七层模型

OSI 七层模型是一种经典理论模型。

| 层级 | 名称 | 大致作用 |
| ---- | ---- | -------- |
| 7 | 应用层 | 应用协议，例如 HTTP、DNS |
| 6 | 表示层 | 数据表示、编码、加密 |
| 5 | 会话层 | 会话管理 |
| 4 | 传输层 | 端到端传输，例如 TCP、UDP |
| 3 | 网络层 | 寻址和路由，例如 IP |
| 2 | 数据链路层 | 局域网数据帧传输 |
| 1 | 物理层 | 电信号、光信号、无线信号 |

基础阶段不需要死记每层细节，但应知道网络通信是分层完成的。

### 14.3 TCP/IP 四层模型

实际开发中更常使用 TCP/IP 模型理解网络。

| 层级 | 名称 | 常见协议或技术 |
| ---- | ---- | -------------- |
| 4 | 应用层 | HTTP、HTTPS、DNS、FTP、SMTP |
| 3 | 传输层 | TCP、UDP |
| 2 | 网络层 | IP、ICMP |
| 1 | 网络接口层 | 以太网、Wi-Fi |

Java 网络编程中经常接触的是：

1. 应用层：HTTP 请求、响应。
2. 传输层：TCP、UDP。
3. 网络层：IP 地址。

### 14.4 数据封装

发送数据时，高层数据会逐层交给低层处理。

```text
HTTP 数据
-> TCP 数据段
-> IP 数据包
-> 网络接口层数据帧
```

接收数据时，再反方向逐层解析。

基础阶段可以这样理解：应用程序关注的是请求和响应，但底层网络会负责把这些数据送到目标设备。

## 15. 常见网络协议

### 15.1 什么是协议

协议是通信双方共同遵守的规则。

如果两个人说不同语言，就很难交流。网络程序也是一样，客户端和服务器必须按照共同协议组织数据。

常见协议：

| 协议 | 所属层次 | 作用 |
| ---- | -------- | ---- |
| IP | 网络层 | 寻址和路由 |
| TCP | 传输层 | 可靠传输 |
| UDP | 传输层 | 无连接传输 |
| HTTP | 应用层 | Web 请求和响应 |
| HTTPS | 应用层 | 加密的 HTTP |
| DNS | 应用层 | 域名解析 |
| FTP | 应用层 | 文件传输 |
| SMTP | 应用层 | 邮件发送 |

### 15.2 TCP

TCP 是面向连接、可靠传输的协议。

特点：

1. 通信前需要建立连接。
2. 能保证数据按顺序到达。
3. 数据丢失时可以重传。
4. 适合需要可靠性的场景。

常见使用场景：

1. 网页访问。
2. 文件传输。
3. 登录系统。
4. 大多数后端接口调用。

### 15.3 UDP

UDP 是无连接、不保证可靠传输的协议。

特点：

1. 不需要先建立连接。
2. 发送开销较小。
3. 不保证一定到达。
4. 不保证顺序。

常见使用场景：

1. 实时音视频。
2. 在线游戏部分数据。
3. 局域网广播。
4. DNS 查询。

基础阶段可以这样理解：TCP 更可靠，UDP 更轻量。

### 15.4 HTTP 和 HTTPS

HTTP 是超文本传输协议，常用于浏览器、接口和后端服务通信。

HTTPS 是在 HTTP 基础上增加加密保护的协议。

常见访问：

```text
http://example.com
https://example.com
```

其中：

1. `http` 默认端口通常是 `80`。
2. `https` 默认端口通常是 `443`。
3. 实际项目中更常使用 `https`。

## 16. 请求和响应

### 16.1 什么是请求

请求是客户端发送给服务器的数据。

请求通常包含：

1. 请求目标。
2. 请求方法。
3. 请求头。
4. 请求参数。
5. 请求体。

示例：

```text
GET /users?id=1 HTTP/1.1
Host: example.com
Accept: application/json
```

可以理解为：客户端告诉服务器“我要访问什么资源，以及我带了哪些信息”。

### 16.2 什么是响应

响应是服务器返回给客户端的数据。

响应通常包含：

1. 状态码。
2. 响应头。
3. 响应体。

示例：

```text
HTTP/1.1 200 OK
Content-Type: application/json

{"id":1,"name":"Alice"}
```

可以理解为：服务器告诉客户端“请求处理结果是什么，以及返回的数据是什么”。

### 16.3 请求和响应的关系

HTTP 通信通常是一次请求对应一次响应。

```text
客户端 -> 请求 -> 服务器
客户端 <- 响应 <- 服务器
```

例如：

1. 浏览器请求首页。
2. 服务器返回 HTML。
3. 浏览器继续请求 CSS、JavaScript、图片。
4. 服务器分别返回对应资源。

## 17. HTTP 请求方法

### 17.1 常见请求方法

HTTP 请求方法表示客户端想对资源做什么操作。

| 方法 | 常见含义 |
| ---- | -------- |
| `GET` | 获取资源 |
| `POST` | 提交数据或创建资源 |
| `PUT` | 整体更新资源 |
| `PATCH` | 局部更新资源 |
| `DELETE` | 删除资源 |
| `HEAD` | 只获取响应头 |
| `OPTIONS` | 查询服务器支持的能力 |

### 17.2 `GET`

`GET` 通常用于获取数据。

示例：

```text
GET /students?classId=1
```

特点：

1. 参数常放在 URL 查询字符串中。
2. 通常不用于提交敏感数据。
3. 多次请求同一个地址通常不应改变服务器数据。

### 17.3 `POST`

`POST` 通常用于提交数据。

示例：

```text
POST /students
Content-Type: application/json

{"name":"Alice","age":18}
```

特点：

1. 数据常放在请求体中。
2. 常用于新增资源、提交表单、登录等场景。
3. 可能改变服务器数据。

### 17.4 `PUT`、`PATCH` 和 `DELETE`

`PUT` 通常表示整体更新。

```text
PUT /students/1
```

`PATCH` 通常表示局部更新。

```text
PATCH /students/1
```

`DELETE` 通常表示删除资源。

```text
DELETE /students/1
```

具体含义还要看后端接口设计。

## 18. URL、URI、查询参数和请求头

### 18.1 URI 和 URL

URI 是统一资源标识符，用于标识一个资源。

URL 是统一资源定位符，用于说明资源的位置。

URL 是 URI 的一种常见形式。

示例：

```text
https://example.com:443/users?id=1
```

可以拆成：

| 部分 | 示例 | 含义 |
| ---- | ---- | ---- |
| 协议 | `https` | 使用 HTTPS |
| 主机 | `example.com` | 目标域名 |
| 端口 | `443` | 目标端口 |
| 路径 | `/users` | 资源路径 |
| 查询参数 | `id=1` | 附加参数 |

### 18.2 查询参数

查询参数写在 `?` 后面，多个参数用 `&` 连接。

```text
/students?classId=1&page=2&size=10
```

其中：

1. `classId=1` 表示班级 ID。
2. `page=2` 表示第 2 页。
3. `size=10` 表示每页 10 条。

### 18.3 请求头

请求头用于携带请求的附加信息。

常见请求头：

| 请求头 | 含义 |
| ------ | ---- |
| `Host` | 目标主机 |
| `Accept` | 客户端希望接收的数据类型 |
| `Content-Type` | 请求体的数据类型 |
| `User-Agent` | 客户端信息 |
| `Authorization` | 身份认证信息 |

示例：

```text
Content-Type: application/json
Authorization: Bearer token
```

### 18.4 请求体

请求体用于携带较复杂的数据。

常见于 `POST`、`PUT`、`PATCH` 请求。

示例 JSON：

```json
{
  "name": "Alice",
  "age": 18
}
```

## 19. 状态码

### 19.1 什么是状态码

状态码是服务器返回给客户端的处理结果编号。

常见分类：

| 范围 | 含义 |
| ---- | ---- |
| `1xx` | 信息提示 |
| `2xx` | 成功 |
| `3xx` | 重定向 |
| `4xx` | 客户端错误 |
| `5xx` | 服务器错误 |

### 19.2 常见状态码

| 状态码 | 含义 |
| ------ | ---- |
| `200` | 请求成功 |
| `201` | 创建成功 |
| `204` | 成功但没有响应体 |
| `301` | 永久重定向 |
| `302` | 临时重定向 |
| `400` | 请求格式错误 |
| `401` | 未认证 |
| `403` | 无权限 |
| `404` | 资源不存在 |
| `500` | 服务器内部错误 |

基础阶段重点掌握 `200`、`400`、`401`、`403`、`404`、`500`。

## 20. Java 网络基础 API

### 20.1 `InetAddress`

`InetAddress` 用于表示 IP 地址或主机地址。

示例：

```java
import java.net.InetAddress;
import java.net.UnknownHostException;

public class InetAddressDemo {
    public static void main(String[] args) {
        try {
            InetAddress local = InetAddress.getLocalHost();
            System.out.println(local.getHostName());
            System.out.println(local.getHostAddress());

            InetAddress address = InetAddress.getByName("localhost");
            System.out.println(address.getHostAddress());
        } catch (UnknownHostException e) {
            System.out.println("主机解析失败：" + e.getMessage());
        }
    }
}
```

其中：

1. `getLocalHost()` 获取本机地址信息。
2. `getByName("localhost")` 根据主机名获取地址。
3. `UnknownHostException` 表示主机解析失败。

### 20.2 `URI`

`URI` 用于表示统一资源标识符。

```java
import java.net.URI;

public class UriDemo {
    public static void main(String[] args) {
        URI uri = URI.create("https://example.com/users?id=1");

        System.out.println(uri.getScheme());
        System.out.println(uri.getHost());
        System.out.println(uri.getPath());
        System.out.println(uri.getQuery());
    }
}
```

输出内容对应协议、主机、路径和查询参数。

### 20.3 `URL`

`URL` 也可以表示资源定位地址。

```java
import java.net.MalformedURLException;
import java.net.URL;

public class UrlDemo {
    public static void main(String[] args) {
        try {
            URL url = new URL("https://example.com/users?id=1");

            System.out.println(url.getProtocol());
            System.out.println(url.getHost());
            System.out.println(url.getPath());
            System.out.println(url.getQuery());
        } catch (MalformedURLException e) {
            System.out.println("URL 格式错误：" + e.getMessage());
        }
    }
}
```

现代 Java 中，如果只是解析和构造地址，通常优先使用 `URI`。真正发送 HTTP 请求时，可以使用 `HttpClient`。

## 21. Java `HttpClient`

### 21.1 `HttpClient` 的含义

`HttpClient` 是 Java 提供的 HTTP 客户端 API。

它位于 `java.net.http` 包中，可以用于发送 HTTP 请求并接收 HTTP 响应。

常用类：

| 类 | 含义 |
| -- | ---- |
| `HttpClient` | HTTP 客户端 |
| `HttpRequest` | HTTP 请求 |
| `HttpResponse` | HTTP 响应 |
| `BodyHandlers` | 响应体处理方式 |
| `BodyPublishers` | 请求体发布方式 |

### 21.2 发送 `GET` 请求

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpGetDemo {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .GET()
                .build();

        try {
            HttpResponse<String> response = client.send(
                    request,
                    HttpResponse.BodyHandlers.ofString()
            );

            System.out.println(response.statusCode());
            System.out.println(response.body());
        } catch (IOException e) {
            System.out.println("网络请求失败：" + e.getMessage());
        } catch (InterruptedException e) {
            System.out.println("请求被中断：" + e.getMessage());
            Thread.currentThread().interrupt();
        }
    }
}
```

其中：

1. `HttpClient.newHttpClient()` 创建客户端。
2. `HttpRequest.newBuilder()` 创建请求构建器。
3. `uri(...)` 指定请求地址。
4. `.GET()` 指定请求方法。
5. `client.send(...)` 发送同步请求。
6. `response.statusCode()` 获取状态码。
7. `response.body()` 获取响应体。

运行该示例需要能够访问互联网。

### 21.3 添加请求头

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpHeaderDemo {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .header("Accept", "text/html")
                .GET()
                .build();

        try {
            HttpResponse<String> response = client.send(
                    request,
                    HttpResponse.BodyHandlers.ofString()
            );

            System.out.println(response.statusCode());
        } catch (IOException e) {
            System.out.println("网络请求失败：" + e.getMessage());
        } catch (InterruptedException e) {
            System.out.println("请求被中断：" + e.getMessage());
            Thread.currentThread().interrupt();
        }
    }
}
```

`header("Accept", "text/html")` 表示告诉服务器客户端希望接收 HTML 文本。

### 21.4 发送 `POST` 请求

```java
import java.io.IOException;
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

public class HttpPostDemo {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        String json = "{\"name\":\"Alice\",\"age\":18}";

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://httpbin.org/post"))
                .header("Content-Type", "application/json")
                .POST(HttpRequest.BodyPublishers.ofString(json))
                .build();

        try {
            HttpResponse<String> response = client.send(
                    request,
                    HttpResponse.BodyHandlers.ofString()
            );

            System.out.println(response.statusCode());
            System.out.println(response.body());
        } catch (IOException e) {
            System.out.println("网络请求失败：" + e.getMessage());
        } catch (InterruptedException e) {
            System.out.println("请求被中断：" + e.getMessage());
            Thread.currentThread().interrupt();
        }
    }
}
```

其中：

1. `Content-Type: application/json` 表示请求体是 JSON。
2. `BodyPublishers.ofString(json)` 表示使用字符串作为请求体。
3. 该示例需要能够访问外部测试服务。

### 21.5 异步请求

`sendAsync` 可以发送异步请求。

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class HttpAsyncDemo {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .GET()
                .build();

        CompletableFuture<HttpResponse<String>> future = client.sendAsync(
                request,
                HttpResponse.BodyHandlers.ofString()
        );

        future.thenAccept(response -> {
            System.out.println(response.statusCode());
            System.out.println(response.body());
        }).join();
    }
}
```

基础阶段重点掌握同步请求即可。异步请求只要求能看懂基本结构。

## 22. TCP 编程

### 22.1 TCP 编程基本思路

Java 中可以使用 `Socket` 和 `ServerSocket` 进行 TCP 编程。

角色：

1. `ServerSocket`：服务器端，用于监听端口，等待客户端连接。
2. `Socket`：客户端或服务器端连接对象，用于读写数据。

基本流程：

```text
服务器创建 ServerSocket 并监听端口
客户端创建 Socket 连接服务器
服务器 accept 接收连接
双方通过输入流和输出流通信
通信结束后关闭资源
```

### 22.2 TCP 服务器

创建 `TcpServerDemo.java`：

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

public class TcpServerDemo {
    public static void main(String[] args) {
        try (ServerSocket serverSocket = new ServerSocket(9000)) {
            System.out.println("服务器已启动，等待客户端连接");

            try (
                    Socket socket = serverSocket.accept();
                    BufferedReader reader = new BufferedReader(
                            new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8)
                    );
                    PrintWriter writer = new PrintWriter(socket.getOutputStream(), true, StandardCharsets.UTF_8)
            ) {
                String message = reader.readLine();
                System.out.println("收到客户端消息：" + message);

                writer.println("服务器已收到：" + message);
            }
        } catch (IOException e) {
            System.out.println("服务器异常：" + e.getMessage());
        }
    }
}
```

### 22.3 TCP 客户端

创建 `TcpClientDemo.java`：

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

public class TcpClientDemo {
    public static void main(String[] args) {
        try (
                Socket socket = new Socket("127.0.0.1", 9000);
                BufferedReader reader = new BufferedReader(
                        new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8)
                );
                PrintWriter writer = new PrintWriter(socket.getOutputStream(), true, StandardCharsets.UTF_8)
        ) {
            writer.println("Hello Server");

            String response = reader.readLine();
            System.out.println("收到服务器响应：" + response);
        } catch (IOException e) {
            System.out.println("客户端异常：" + e.getMessage());
        }
    }
}
```

运行顺序：

1. 先运行 `TcpServerDemo`。
2. 再运行 `TcpClientDemo`。
3. 观察服务器端和客户端输出。

### 22.4 TCP 代码说明

服务器端：

1. `new ServerSocket(9000)` 表示监听 `9000` 端口。
2. `serverSocket.accept()` 会等待客户端连接。
3. `socket.getInputStream()` 读取客户端发送的数据。
4. `socket.getOutputStream()` 向客户端返回数据。

客户端：

1. `new Socket("127.0.0.1", 9000)` 表示连接本机 `9000` 端口。
2. `writer.println(...)` 向服务器发送一行文本。
3. `reader.readLine()` 读取服务器返回的一行文本。

### 22.5 多客户端问题

上面的 TCP 服务器一次只能处理一个客户端连接，处理完后程序结束。

如果希望服务器一直运行，可以使用循环。

```java
while (true) {
    Socket socket = serverSocket.accept();
    // 处理客户端
}
```

如果希望同时处理多个客户端，通常需要配合多线程或线程池。线程内容不在本任务中展开，只要求知道真实服务器通常不会只处理一个客户端。

## 23. UDP 编程

### 23.1 UDP 编程基本思路

Java 中可以使用 `DatagramSocket` 和 `DatagramPacket` 进行 UDP 编程。

角色：

1. `DatagramSocket`：用于发送或接收 UDP 数据。
2. `DatagramPacket`：表示一个 UDP 数据包。

UDP 不需要先建立连接，发送方直接把数据包发给目标 IP 和端口。

### 23.2 UDP 接收端

创建 `UdpReceiverDemo.java`：

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.nio.charset.StandardCharsets;

public class UdpReceiverDemo {
    public static void main(String[] args) {
        byte[] buffer = new byte[1024];

        try (DatagramSocket socket = new DatagramSocket(9001)) {
            System.out.println("UDP 接收端已启动");

            DatagramPacket packet = new DatagramPacket(buffer, buffer.length);
            socket.receive(packet);

            String message = new String(
                    packet.getData(),
                    0,
                    packet.getLength(),
                    StandardCharsets.UTF_8
            );

            System.out.println("收到消息：" + message);
            System.out.println("发送方地址：" + packet.getAddress().getHostAddress());
            System.out.println("发送方端口：" + packet.getPort());
        } catch (IOException e) {
            System.out.println("接收失败：" + e.getMessage());
        }
    }
}
```

### 23.3 UDP 发送端

创建 `UdpSenderDemo.java`：

```java
import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;
import java.nio.charset.StandardCharsets;

public class UdpSenderDemo {
    public static void main(String[] args) {
        byte[] data = "Hello UDP".getBytes(StandardCharsets.UTF_8);

        try (DatagramSocket socket = new DatagramSocket()) {
            DatagramPacket packet = new DatagramPacket(
                    data,
                    data.length,
                    InetAddress.getByName("127.0.0.1"),
                    9001
            );

            socket.send(packet);
            System.out.println("UDP 消息已发送");
        } catch (IOException e) {
            System.out.println("发送失败：" + e.getMessage());
        }
    }
}
```

运行顺序：

1. 先运行 `UdpReceiverDemo`。
2. 再运行 `UdpSenderDemo`。
3. 观察接收端输出。

### 23.4 TCP 和 UDP 的区别

| 对比项 | TCP | UDP |
| ------ | --- | --- |
| 是否连接 | 需要连接 | 不需要连接 |
| 是否可靠 | 可靠 | 不保证可靠 |
| 顺序 | 保证顺序 | 不保证顺序 |
| 开销 | 较高 | 较低 |
| Java 类 | `Socket`、`ServerSocket` | `DatagramSocket`、`DatagramPacket` |

基础阶段可以这样选择：

1. 需要可靠传输时，优先理解 TCP。
2. 需要轻量、实时、允许少量丢包时，可以了解 UDP。

## 24. 网络编程中的异常和资源

### 24.1 常见异常

网络编程常见异常：

| 异常 | 常见原因 |
| ---- | -------- |
| `UnknownHostException` | 域名或主机无法解析 |
| `ConnectException` | 连接失败，目标服务未启动或端口不通 |
| `SocketTimeoutException` | 连接或读取超时 |
| `BindException` | 端口已被占用 |
| `IOException` | 网络读写失败 |
| `InterruptedException` | HTTP 请求等待过程中线程被中断 |

示例：

```java
try {
    Socket socket = new Socket("127.0.0.1", 9999);
} catch (IOException e) {
    System.out.println("连接失败：" + e.getMessage());
}
```

### 24.2 超时设置

网络请求不应无限等待。

`HttpClient` 请求超时：

```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.time.Duration;

public class HttpTimeoutDemo {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newBuilder()
                .connectTimeout(Duration.ofSeconds(5))
                .build();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://example.com"))
                .timeout(Duration.ofSeconds(10))
                .GET()
                .build();

        System.out.println(client);
        System.out.println(request);
    }
}
```

`Socket` 读取超时：

```java
import java.io.IOException;
import java.net.Socket;

public class SocketTimeoutDemo {
    public static void main(String[] args) {
        try (Socket socket = new Socket("127.0.0.1", 9000)) {
            socket.setSoTimeout(5000);
            System.out.println("连接成功");
        } catch (IOException e) {
            System.out.println("连接或读取失败：" + e.getMessage());
        }
    }
}
```

### 24.3 关闭资源

网络编程中常见需要关闭的资源：

1. `Socket`。
2. `ServerSocket`。
3. `DatagramSocket`。
4. 输入流。
5. 输出流。

推荐使用 `try-with-resources`。

```java
try (Socket socket = new Socket("127.0.0.1", 9000)) {
    System.out.println("连接成功");
} catch (IOException e) {
    System.out.println("连接失败：" + e.getMessage());
}
```

关闭 `Socket` 时，相关输入流和输出流通常也会被关闭。

## 25. 代码示例

在 IDEA 中创建以下文件，并保证它们位于同一个包或同一个源码目录下。

### 25.1 `Route.java`

```java
import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Route {
    String value();
}
```

### 25.2 `MessageController.java`

```java
public class MessageController {
    @Route("/hello")
    public String hello() {
        return "Hello Reflection";
    }

    @Route("/time")
    public String time() {
        return String.valueOf(System.currentTimeMillis());
    }
}
```

### 25.3 `SimpleDispatcher.java`

```java
import java.lang.reflect.Method;
import java.util.HashMap;
import java.util.Map;

public class SimpleDispatcher {
    private Object controller;
    private Map<String, Method> routeMap = new HashMap<>();

    public SimpleDispatcher(Object controller) {
        this.controller = controller;

        Method[] methods = controller.getClass().getDeclaredMethods();

        for (Method method : methods) {
            Route route = method.getAnnotation(Route.class);

            if (route != null) {
                routeMap.put(route.value(), method);
            }
        }
    }

    public String dispatch(String path) {
        Method method = routeMap.get(path);

        if (method == null) {
            return "404 Not Found";
        }

        try {
            Object result = method.invoke(controller);
            return String.valueOf(result);
        } catch (ReflectiveOperationException e) {
            return "500 Server Error: " + e.getMessage();
        }
    }
}
```

### 25.4 `Task7Demo.java`

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.ServerSocket;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

public class Task7Demo {
    public static void main(String[] args) {
        SimpleDispatcher dispatcher = new SimpleDispatcher(new MessageController());

        System.out.println(dispatcher.dispatch("/hello"));
        System.out.println(dispatcher.dispatch("/time"));
        System.out.println(dispatcher.dispatch("/none"));

        try (ServerSocket serverSocket = new ServerSocket(9002)) {
            System.out.println("简单 TCP 服务已启动，端口 9002");
            System.out.println("请使用 TCP 客户端发送 /hello 或 /time");

            try (
                    Socket socket = serverSocket.accept();
                    BufferedReader reader = new BufferedReader(
                            new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8)
                    );
                    PrintWriter writer = new PrintWriter(socket.getOutputStream(), true, StandardCharsets.UTF_8)
            ) {
                String path = reader.readLine();
                String response = dispatcher.dispatch(path);

                writer.println(response);
            }
        } catch (IOException e) {
            System.out.println("网络服务异常：" + e.getMessage());
        }
    }
}
```

### 25.5 `Task7Client.java`

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.io.PrintWriter;
import java.net.Socket;
import java.nio.charset.StandardCharsets;

public class Task7Client {
    public static void main(String[] args) {
        try (
                Socket socket = new Socket("127.0.0.1", 9002);
                BufferedReader reader = new BufferedReader(
                        new InputStreamReader(socket.getInputStream(), StandardCharsets.UTF_8)
                );
                PrintWriter writer = new PrintWriter(socket.getOutputStream(), true, StandardCharsets.UTF_8)
        ) {
            writer.println("/hello");

            String response = reader.readLine();
            System.out.println(response);
        } catch (IOException e) {
            System.out.println("客户端异常：" + e.getMessage());
        }
    }
}
```

运行后应重点观察以下内容：

1. `@Route` 注解如何标记方法。
2. `SimpleDispatcher` 如何通过反射扫描方法。
3. `Method` 如何保存方法信息。
4. `method.invoke` 如何调用控制器方法。
5. `HashMap<String, Method>` 如何建立路径和方法之间的关系。
6. `ServerSocket` 如何监听端口。
7. `Socket` 如何连接服务器。
8. 客户端发送路径后，服务器如何返回对应结果。
9. 这个示例如何把反射和网络请求的思想联系起来。

## 26. 练习

### 26.1 `Class` 对象练习

创建 `ReflectionClassPractice.java`，完成以下要求：

1. 定义 `Student` 类，包含 `name` 和 `age`。
2. 分别使用 `Student.class`、对象 `.getClass()`、`Class.forName` 获取 `Class` 对象。
3. 比较三个 `Class` 对象是否相同。
4. 输出类的完整类名。
5. 输出类的简单类名。
6. 输出类的包名。
7. 输出父类名称。
8. 写注释说明 `Class` 对象的含义。

### 26.2 构造方法、字段和方法反射练习

创建 `ReflectionMemberPractice.java`，完成以下要求：

1. 定义 `Product` 类，包含 `private String name` 和 `private double price`。
2. 定义无参构造方法和有参构造方法。
3. 定义 `public void showInfo()` 方法。
4. 使用反射获取所有构造方法并输出。
5. 使用反射调用有参构造方法创建 `Product` 对象。
6. 使用反射获取 `name` 字段。
7. 使用 `setAccessible(true)` 修改 `name` 字段。
8. 使用反射调用 `showInfo` 方法。
9. 输出字段和方法的修饰符。

### 26.3 注解反射练习

创建 `Column.java`、`Table.java` 和 `AnnotationPractice.java`，完成以下要求：

1. 定义 `@Table` 注解，可用于类上，并保留到运行时。
2. 定义 `@Column` 注解，可用于字段上，并保留到运行时。
3. 定义 `UserEntity` 类，使用 `@Table("users")`。
4. 在 `UserEntity` 的字段上使用 `@Column`。
5. 使用反射读取类上的 `@Table` 注解。
6. 使用反射读取字段上的 `@Column` 注解。
7. 输出类名、表名、字段名和列名。

### 26.4 动态代理练习

创建 `Service.java`、`ServiceImpl.java` 和 `ProxyPractice.java`，完成以下要求：

1. 定义 `Service` 接口，包含 `void execute(String task)` 方法。
2. 定义 `ServiceImpl` 实现 `Service`。
3. 使用 `Proxy.newProxyInstance` 创建动态代理对象。
4. 在方法调用前输出 `开始执行`。
5. 调用真实对象的方法。
6. 在方法调用后输出 `执行结束`。
7. 写注释说明动态代理适合做什么。

### 26.5 网络概念练习

创建 `NetworkConceptNote.md`，完成以下要求：

1. 用自己的话说明什么是网络。
2. 说明客户端和服务器的区别。
3. 说明 IP 地址的作用。
4. 说明端口的作用。
5. 说明域名和 DNS 的关系。
6. 画出一次 HTTP 请求和响应的简单流程。
7. 列出 TCP/IP 四层模型。
8. 说明 TCP 和 UDP 的区别。
9. 列出至少 5 个常见 HTTP 状态码及含义。
10. 列出 `GET`、`POST`、`PUT`、`PATCH`、`DELETE` 的常见用途。

### 26.6 `InetAddress`、`URI` 和 `URL` 练习

创建 `NetworkApiPractice.java`，完成以下要求：

1. 使用 `InetAddress.getLocalHost` 获取本机主机名和 IP。
2. 使用 `InetAddress.getByName("localhost")` 获取本机地址。
3. 使用 `URI.create` 创建一个包含协议、主机、路径、查询参数的 URI。
4. 输出 URI 的协议、主机、路径和查询参数。
5. 创建一个 `URL` 对象并输出协议、主机、路径和查询参数。
6. 捕获 `UnknownHostException`。
7. 捕获 `MalformedURLException`。

### 26.7 `HttpClient` 练习

创建 `HttpClientPractice.java`，完成以下要求：

1. 创建 `HttpClient` 对象。
2. 创建 `GET` 请求访问 `https://example.com`。
3. 输出响应状态码。
4. 输出响应体前 200 个字符。
5. 给请求添加 `Accept` 请求头。
6. 创建一个 `POST` 请求，发送 JSON 字符串。
7. 设置请求超时时间。
8. 捕获 `IOException` 和 `InterruptedException`。
9. 在捕获 `InterruptedException` 后调用 `Thread.currentThread().interrupt()`。

### 26.8 TCP 练习

创建 `TcpServerPractice.java` 和 `TcpClientPractice.java`，完成以下要求：

1. 服务器使用 `ServerSocket` 监听 `9100` 端口。
2. 客户端使用 `Socket` 连接 `127.0.0.1:9100`。
3. 客户端向服务器发送一行文本。
4. 服务器读取客户端文本并输出。
5. 服务器向客户端返回一行响应。
6. 客户端读取服务器响应并输出。
7. 使用 UTF-8 编码处理文本。
8. 使用 `try-with-resources` 关闭资源。
9. 捕获 `IOException`。

### 26.9 UDP 练习

创建 `UdpReceiverPractice.java` 和 `UdpSenderPractice.java`，完成以下要求：

1. 接收端使用 `DatagramSocket` 监听 `9101` 端口。
2. 发送端使用 `DatagramSocket` 发送消息。
3. 使用 `DatagramPacket` 封装消息。
4. 消息发送到 `127.0.0.1:9101`。
5. 使用 UTF-8 编码转换字符串和字节数组。
6. 接收端输出收到的消息。
7. 接收端输出发送方 IP 和端口。
8. 使用 `try-with-resources` 关闭资源。

### 26.10 综合练习

创建 `Task7Practice.java`，完成以下要求：

1. 定义 `@RequestPath` 注解，可用于方法，并保留到运行时。
2. 定义 `StudentController` 类。
3. 在 `StudentController` 中定义 `list`、`detail`、`create` 三个方法。
4. 分别使用 `@RequestPath("/students")`、`@RequestPath("/students/detail")`、`@RequestPath("/students/create")` 标记方法。
5. 定义 `Dispatcher` 类，使用反射扫描 `StudentController` 中带注解的方法。
6. 使用 `HashMap<String, Method>` 保存路径和方法的对应关系。
7. 定义 `dispatch(String path)` 方法，根据路径调用对应方法。
8. 路径不存在时返回 `404 Not Found`。
9. 方法调用出错时返回 `500 Server Error`。
10. 使用 `ServerSocket` 创建一个简单 TCP 服务，监听 `9200` 端口。
11. 客户端发送一行路径，例如 `/students`。
12. 服务器调用 `Dispatcher` 并把结果返回给客户端。
13. 创建 `Task7PracticeClient.java` 测试至少两个路径。
14. 使用 `try-with-resources` 关闭网络资源。
15. 使用注释说明本程序中哪里使用了反射，哪里体现了请求和响应。

## 27. 验收标准

满足以下条件视为完成 Task 7：

1. 能说明反射的含义。
2. 能说明 `Class` 对象的作用。
3. 能使用三种方式获取 `Class` 对象。
4. 能通过反射获取类的基本信息。
5. 能通过反射获取并调用构造方法。
6. 能通过反射获取、读取和修改字段。
7. 能通过反射获取并调用方法。
8. 能说明 `setAccessible(true)` 的作用和风险。
9. 能定义基础注解并设置运行时保留。
10. 能通过反射读取类和字段上的注解。
11. 能看懂 JDK 动态代理的基本结构。
12. 能说明反射的常见使用场景和缺点。
13. 能说明网络、客户端和服务器的含义。
14. 能说明 IP 地址、端口、域名和 DNS 的作用。
15. 能说明 OSI 七层模型和 TCP/IP 四层模型的基本思想。
16. 能说明 TCP、UDP、HTTP、HTTPS、DNS 的基本作用。
17. 能说明请求和响应的含义。
18. 能说明常见 HTTP 请求方法。
19. 能说明 URL、URI、查询参数、请求头和请求体的含义。
20. 能识别常见 HTTP 状态码。
21. 能使用 `InetAddress` 获取主机地址信息。
22. 能使用 `URI` 和 `URL` 解析地址信息。
23. 能使用 `HttpClient` 发送基础 HTTP 请求。
24. 能使用 `ServerSocket` 和 `Socket` 完成基础 TCP 通信。
25. 能使用 `DatagramSocket` 和 `DatagramPacket` 完成基础 UDP 通信。
26. 能说明网络编程中的常见异常。
27. 能使用 `try-with-resources` 管理网络资源。
28. 能独立完成 `Task7Practice.java` 和 `Task7PracticeClient.java` 并运行得到正确结果。

## 28. 提交规范

1. 学习/实验总结报告统一命名为 `Task_7_report.md`，放置在 `Task_7/` 目录下。
2. Java 源码或工程文件统一放置在 `Task_7/src/` 目录下（或在 `Task_7/` 下使用独立的 Maven/Gradle 工程结构）。
3. 严禁提交 `target/`、`build/`、`out/`、`*.class`、`*.jar`、`.idea/`、`*.iml`、`.vscode/`、`.settings/`、`.DS_Store` 和 `Thumbs.db` 等本地缓存、IDE 配置及构建产物。
4. 单个文件不得超过 100MB，不要提交大型测试数据集、视频或依赖压缩包。
5. 提交前请确认改动符合仓库根目录 `README.md` 和 `.gitignore` 的要求，并仅提交 `Task_7/` 目录中的本阶段内容。
