# Java

欢迎加入 Epoch Lab！本项目为你的Task仓库。仓库已按阶段拆分为不同的任务模块（Task_0 ~ Task_8），请按要求在对应目录下完成开发并提交学习总结。

---

## 一、 目录组织与交付规范

每个 `Task_x` 目录下均包含该阶段的任务说明文件（`Task_x.md`）。请严格按照以下层级在对应任务文件夹内补充你的报告与代码：

```text
Epoch Task/
├── Task_0/
│   ├── Task_0.md           # 任务要求与说明
│   ├── report.md           # 你的学习总结报告
│   ├── img/                # 你的学习总结报告中所引用的图片
│   └── src/                # 本任务相关的 Java 源码（或工程目录）
├── Task_1/
│   ├── Task_1.md
│   ├── report.md
│   ├── img/
│   └── src/
├── ...
├── Task_8/
│   ├── Task_8.md
│   ├── report.md
│   ├── img/
│   └── src/
├── .gitignore              
└── README.md               

```

> **说明**：
> 1. 每个任务的总结报告统一命名为 `Task_x_report.md`，放置在对应的 `Task_x/` 目录下。
> 2. 如需提交配套 Java 源码，请统一存放于该任务对应的 `src/` 目录下（或该任务下独立的 Maven/Gradle 工程结构中）。
> 
> 

---

## 二、 提交与过滤规范

仓库根目录已配置 `.gitignore`。**严禁提交任何本地缓存、IDE 配置及构建产物**：

* **严禁上传**：
* 构建与编译产物：`target/`, `build/`, `out/`, `*.class`, `*.jar`
* IDE 配置文件：`.idea/`, `*.iml`, `.vscode/`, `.settings/`
* 系统缓存文件：`.DS_Store`, `Thumbs.db`


* **文件体积限制**：GitHub 严禁单个文件超过 100MB（超过 50MB 告警）。请不要将大型测试数据集、视频或依赖压缩包直接提交到 Git 仓库中。

---

## 三、 提交流程指引

### 方式 1：本地 Git 命令行

1. **克隆本仓库到本地**：
```bash
git clone [https://github.com/Epoch-Academy/](https://github.com/Epoch-Academy/)<你的仓库名>.git
cd <你的仓库名>

```


2. **按阶段完成任务并提交**：
```bash
# 查看当前改动状态，确认没有引入 target/ 或 .idea/
git status

# 暂存当前阶段完成的任务（以 Task_0 为例）
git add Task_0/

# 提交并推送到 GitHub
git commit -m "feat: complete Task_0 code and report"
git push origin main

```



### 方式 2：GitHub 网页端上传

若本地 Git 环境暂未配置完善，可通过网页端直接交付：

1. 点击进入对应的任务目录（如 `Task_0/`）。
2. 点击右上角 **Add file** $\rightarrow$ **Upload files**。
3. 拖拽放入你编写的 `report.md` 以及代码文件。
4. 页面底部填写 Commit 说明，点击 **Commit changes**。

---

## 四、 评审与成绩反馈

1. **阶段截止**：各阶段任务的截止时间以实验室群通知为准，逾期提交将记录迟交。
2. **离线评阅**：负责人将定期克隆本仓库，阅读 `report.md` 并在本地验证 `src/` 代码实现。
3. **查分机制**：批改完成后，负责人会直接在当前仓库的 **Issues** 中发布评分与改进建议，请留意邮件与 GitHub 站内提醒。
