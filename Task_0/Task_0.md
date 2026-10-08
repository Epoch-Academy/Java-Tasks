# Task 0：Java 开发环境基础与首个程序运行

## 1. 任务说明

本任务用于建立 Java 学习的基础运行条件。核心内容包括文件系统规范、命令行基础、JDK 安装与验证、IDEA 项目创建与程序运行。

在后续学习中，多数初学阶段问题并非来自语言本身，而来自环境配置不当、路径命名不规范、工具使用流程不清晰。完成本任务后，学习者应具备独立完成基础环境部署与排错的能力。

## 2. 知识范围

本任务包含以下内容：

1. 常见开发文件类型。
2. 本地开发目录规范。
3. Windows 终端基础命令。
4. JDK 安装、环境变量配置与版本验证。
5. IntelliJ IDEA 项目创建与程序运行。
6. Markdown 语法。

## 3. 基础概念

### 3.1 文件类型与后缀

开发场景中，文件后缀用于标识文件性质。至少应准确识别以下类型：

1. `.java`：Java 源代码文件。
2. `.class`：Java 源代码编译后的字节码文件。
3. `.jar`：Java 程序或依赖库的打包文件。
4. `.md`：Markdown 文档文件。
5. `.zip` 或 `.7z`：压缩包文件。

文件后缀不可隐藏。若系统未显示扩展名，应在资源管理器中开启显示选项，以避免错误识别文件。

### 3.2 开发目录规范

推荐建立统一开发根目录，示例如下：

```text
D:\DevSpace
├── Environment
├── Projects
└── Tools
```

目录功能定义如下：

1. `Environment`：存放 JDK 等运行环境。
2. `Projects`：存放课程项目与练习代码。
3. `Tools`：存放压缩工具、辅助软件与安装包。

命名规范如下：

1. 使用英文字符。
2. 不使用空格。
3. 不使用特殊符号。
4. 避免中文路径。

该规范用于降低环境变量配置、命令行执行与跨工具调用中的路径错误率。

### 3.3 终端与命令行基础

Windows 常见终端环境包括 CMD 与 PowerShell。无论具体终端类型，均应掌握以下命令行为：

1. `cd`：切换当前目录。
2. `cd ..`：返回上一级目录。
3. `cd \`：返回当前盘符根目录。
4. `dir`（CMD）或 `Get-ChildItem`（PowerShell）：列出当前目录内容。

示例流程：

```bat
cd \
cd Users
cd <用户名>
cd Desktop
dir
```

学习重点在于理解“当前目录”概念，并能够根据目录层级进行准确导航。

### 3.4 JDK 与 IDEA 的角色分工

JDK（Java Development Kit）提供 Java 程序的编译与运行能力；IDEA 是集成开发环境（IDE），用于代码编辑、项目管理、运行调试。

两者关系如下：

1. JDK 是运行基础。
2. IDEA 是开发工具。
3. IDEA 必须绑定有效 JDK 才能正确创建与运行 Java 项目。

## 4. 操作流程

### 4.1 文件系统准备

1. 在资源管理器开启文件扩展名显示。
2. 建立规范开发目录（`Environment`、`Projects`、`Tools`）。
3. 使用压缩工具将任意压缩包解压至 `Tools`，区分压缩包与解压后目录。

### 4.2 命令行基础验证

1. 打开 Windows Terminal。
2. 从根目录导航至桌面目录。
3. 执行 `dir` 或 `Get-ChildItem` 查看桌面文件。

### 4.3 JDK 21 安装与环境变量配置

JDK 安装目录示例：

```text
D:\DevSpace\Environment\jdk-21
```

环境变量示例：

```text
JAVA_HOME = D:\DevSpace\Environment\jdk-21
Path 追加 = %JAVA_HOME%\bin
```

配置完成后，重新打开终端并执行：

```bat
javac -version
java -version
```

若返回 21 开头版本号，可判定 JDK 基础配置有效。

### 4.4 IDEA 项目创建与程序运行

在 IDEA 中完成以下操作：

1. 创建 Java 项目并绑定 JDK 21。
2. 创建 `HelloWorld.java`。
3. 输入并运行以下程序：

```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```

控制台输出 `Hello World!` 表明项目创建、编译与运行链路已打通。

## 5. 常见问题与排查路径

### 5.1 终端无法识别 `javac`

按以下顺序排查：

1. `JAVA_HOME` 是否指向 JDK 根目录，而非 `bin` 目录。
2. `Path` 是否追加 `%JAVA_HOME%\bin`。
3. 修改后是否已重开终端。
4. JDK 路径是否存在中文、空格或层级错误。

### 5.2 IDEA 无法选择 JDK

1. 在 `Project Structure` 手动添加本地 JDK。
2. 确认所选目录为完整 JDK 目录。
3. 排除误选 JRE 或空目录的情况。

### 5.3 程序编译失败或运行失败

1. 检查文件名与 `public class` 名称是否一致。
2. 检查分号和大括号是否完整配对。
3. 检查 `System.out.println` 的大小写与拼写是否正确。

## 6. Markdown 基础语法

Markdown 是一种轻量级文本标记语法，常用于编写学习记录、项目说明和任务提交文档。Markdown 文件通常使用 `.md` 后缀。

### 6.1 标题

标题使用 `#` 表示，`#` 数量越多，标题层级越低。

```markdown
# 一级标题
## 二级标题
### 三级标题
```

标题符号 `#` 后应保留一个空格。

### 6.2 有序列表与无序列表

有序列表使用数字编号：

```markdown
1. 第一步
2. 第二步
3. 第三步
```

无序列表使用 `-`：

```markdown
- Java
- JDK
- IDEA
```

列表内容应保持缩进和编号清晰。

### 6.3 行内代码与代码块

行内代码使用一对反引号包裹，适合标记命令、文件名、路径或简短代码。

```markdown
执行 `java -version` 查看 Java 版本。
```

多行代码使用三个反引号包裹，并建议写明语言类型。

````markdown
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello World!");
    }
}
```
````

命令行示例可以使用 `bat`、`powershell` 或 `text` 作为代码块类型。

### 6.4 链接与图片

链接语法如下：

```markdown
[显示文本](链接地址)
```

图片语法如下：

```markdown
![图片说明](图片路径)
```

如果图片与 Markdown 文件位于同一目录，可以直接使用图片文件名。

```markdown
![JDK 版本截图](jdk-version.png)
```

### 6.5 文档书写要求

Markdown 学习记录应满足以下要求：

1. 使用标题划分结构。
2. 使用列表整理步骤和结论。
3. 使用代码块保存命令、代码和报错信息。
4. 截图文件名应与文档中引用的图片路径一致。
5. 避免整篇文档只有截图而没有文字说明。

**在接下来的Task中，不要在需提交的Markdown文档中引入图片**

## 7. 学习要求

阅读完本任务后，应按以下顺序完成：

1. 根据第 3 部分理解文件类型、目录规范、终端、JDK 与 IDEA 的基本概念。
2. 根据第 4 部分完成文件系统准备、命令行验证、JDK 配置和 IDEA 程序运行。
3. 在操作过程中保存必要截图，包括目录结构、终端命令结果、JDK 版本信息和 IDEA 运行结果。
4. 根据第 6 部分编写 Markdown 学习记录，记录环境配置、关键概念和实际排错过程。
5. 对照第 8 部分检查任务是否完成。
6. 根据第 9 部分整理提交材料，并按要求命名文件。


## 8. 验收标准

满足以下条件视为完成 Task 0：

1. 能准确区分 `.java`、`.class`、`.jar` 文件。
2. 本地开发目录结构符合命名规范。
3. 能在终端完成根目录到桌面的目录导航并列出文件。
4. `javac -version` 与 `java -version` 输出有效版本号。
5. IDEA 项目绑定 JDK 21 并成功运行 `HelloWorld.java`。
6. 学习总结文档内容完整、结构清晰。

## 9. 提交规范

### 9.1 截图材料

1. 终端目录切换与文件列表截图。
2. 压缩包解压后目录结构截图。
3. `javac -version` 与 `java -version` 截图。
4. IDEA 项目结构与 JDK 配置截图。
5. `HelloWorld.java` 代码与运行输出同屏截图。

### 9.2 文档材料

提交 Markdown 文档，文件名：`Task_0_report.md`。

文档应包含以下内容：

1. 本次开发目录结构。
2. `.java`、`.class`、`.jar` 的作用说明。
3. JDK 与 IDEA 的角色说明。
4. `JAVA_HOME` 与 `Path` 的作用说明。
5. 一次实际问题及其排查过程。
6. 使用过的检索关键词或提问描述。

### 9.3 命名规范

1. 普通截图：`材料序号.jpg/png`。
2. 程序运行截图：`Hello_World.jpg/png`。
