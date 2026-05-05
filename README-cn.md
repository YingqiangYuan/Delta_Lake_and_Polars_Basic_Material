# Delta Lake + Polars 入门教程

## 这门课学什么？

**Delta Lake** 是数据湖（Lakehouse）三驾马车之一，拥有 Python 和 Rust 客户端，性能极强。**Polars** 是新一代高性能 DataFrame 库，用 Rust 编写，速度远超 Pandas。两者结合，可以在不依赖 Spark 的前提下，用纯 Python 完成云端大数据湖的读写、合并、时间旅行、Schema 演进等核心操作。

这是**云计算时代大数据数据仓库的基础技能**。无论你将来做数据工程、数据分析还是机器学习，理解 Delta Lake + Polars 的工作方式都会让你受益匪浅。

### 学完这门课你会掌握

- **Delta Lake 核心操作** — 在 S3 上创建、读写 Delta 表，理解 overwrite / append / error 三种写入模式
- **Polars 数据处理** — 列操作、去重、聚合、Join、PII 脱敏等常见 ETL 手法
- **Merge / Upsert** — 增量更新数据的核心模式，理解 predicate 和 action 的设计
- **时间旅行** — 按版本号回溯历史数据，查看 commit log
- **Vacuum 与 Schema Evolution** — 清理过期文件、安全地演进表结构
- **完整 ETL 管道** — 从 Bronze（原始数据）到 Silver（清洗后数据）的端到端流程

本教程面向入门人群，通过一系列小而完整的 POC 脚本，带你从零掌握这些核心概念。


## 前置课程

我们默认你已经完成以下三门前置课程：

1. **AWS 基础** — 你已经知道如何创建 AWS 账号、配置 AWS CLI credential（`aws configure`）
2. **Claude Code 系列教程** — 你已经知道如何使用 Claude Code 的 slash command（`/` 命令）来与 AI 协作学习
3. **mise-en-place** — 你已经知道这是在电脑上最快准备好开发环境的工具

如果以上任何一项你还不熟悉，请先去完成对应的前置课程再回来。


## 环境准备

### 第一步：安装依赖

只需两条命令：

```bash
mise install
mise run inst
```

如果跑不起来，进入 Claude Code，输入 `/learn-this-project` 命令，让 AI 帮助你在 MacBook 或 GitHub Codespace 上把环境跑起来。

### 第二步：配置 AWS

因为我们直接使用云端 S3 存储（不用本地文件），你需要配置 AWS profile：

1. 复制 `.env.example` 为 `.env`：
   ```bash
   cp .env.example .env
   ```
2. 编辑 `.env`，填入你自己的 AWS profile 名称：
   ```
   AWS_PROFILE="your-profile-name"
   ```

准备工作到此完成。


## 如何学习

### 课程内容结构

`examples/` 目录下有大量练习脚本，每个知识点都被拆解成独立的小脚本。**每个子文件夹下都有一个 `README.md`**，是该模块的小教程，讲解这个模块的背景知识和每个脚本在干什么：

```
examples/
├── 00-minimal-poc/      # 最小可运行示例 + 背景概念
├── 01-delta-read-write/ # Delta Lake 基本读写
├── 02-polars-etl/       # Polars 数据处理
├── 03-merge-upsert/     # 合并/更新操作
├── 04-time-travel/      # 时间旅行（版本回溯）
├── 05-vacuum-schema/    # 清理旧文件 + Schema 演进
└── 06-full-etl/         # 完整的 Bronze→Silver 管道
```

你需要一个一个地用 `python` 运行这些脚本。如果不会运行，问 AI。

### 用 AI 辅助学习

**核心命令：** 进入 Claude Code，输入：

```
/learn-this-project
```

这个命令会加载本教程的所有信息，AI 将清楚地知道如何引导你学习。它有两种模式：

- **Guided Tour（引导学习）** — AI 会从头到尾带你走一遍项目，逐步讲解每个脚本
- **Quiz（练习测验）** — AI 会针对你学过的脚本进行概念提问，帮你巩固理解

### 推荐学习方式

我建议你**同时打开两个窗口**：

1. **窗口 1** — 学习模式：打 `/learn-this-project`，跟着 AI 学
2. **窗口 2** — 练习模式：打 `/learn-this-project`，选 Quiz，一边学一边练

学习过程中，请务必：

- **自己运行代码** — 不要只看不跑，亲手 `python examples/00-minimal-poc/s01_minimal_poc.py` 执行
- **去 S3 上看** — 登录 AWS Console，打开你的 S3 bucket，看看运行脚本后到底发生了什么
- **下载文件体验** — 把 S3 上的文件下载下来看看

关于 S3 上的数据文件：它们是 **Parquet** 格式。Parquet 文件不是给人直接读的，是给机器读的，因为它性能极强。它本质就是个表格，只不过速度性能特别高。如果你想读取 Parquet 文件的内容，可以问 AI 帮你写个脚本来读。

### 如何检验自己学懂了

学习就是一个**学 → 检验 → 再学**不断提升的过程。检验标准：

1. **代码都能跑** — 每个脚本你都亲手运行过，理解输出结果
2. **Quiz 80% 以上能答** — 如果面试中问你这些问题，你能直接用自然语言描述清楚（不需要背代码，但要能讲明白原理和为什么这样做）

如果 Quiz 中有答不上来的，回到对应的脚本重新学习，搞懂了再继续。

### 学习节奏

这个虽然只有一节课，但内容很多，你可以慢慢做，不要急。

多多提问题。遇到不懂的：

- **事实性问题**（"这个 API 参数是什么意思？""这行代码干了啥？"）→ 直接问 AI，AI 回答了就行
- **开放性问题**（"这个架构设计合理吗？""实际项目中应该怎么选？""这个方案的 trade-off 是什么？"）→ 记下来问导师

学会鉴别这两类问题，是一项重要的职业能力。

### 复习

学完之后，你可以随时再用 `/learn-this-project` 的 Quiz 模式来复习巩固。


## 展示你的学习成果

🚨 **重要：请认真阅读本节，这是你的 actionable item！** 🚨

学完之后，我们希望你把学习过程展示在 GitHub 上，让别人知道你是一个循序渐进、持续学习的人。

### 创建你自己的仓库

本教程代码库是 private 的。你需要：

1. 把本项目下载到本地
2. 创建一个**新的 public 仓库**

仓库命名建议：不要照搬 `learn_delta_lake_and_polars_basic`。加上你自己的名字，去掉 `learn`，加上 `POC`。例如：

```
firstname-lastname-delta-lake-and-polars-basic-poc
```

当然你也可以自己随便起一个个性化的名字，不要跟大家撞车。

### 分步提交（非常重要）

🚨 **不要把所有文件一股脑一个 commit 全部塞进去！** 🚨

你应该把它作为一个**渐进式学习过程**来展示。建议：

1. 先提交根目录下的配置文件（`mise.toml`、`pyproject.toml` 等）
2. 再提交 `.claude/` 目录
3. 再提交源代码 `delta_lake_and_polars_basic/`
4. `examples/` 里面的子文件夹和文件，**一个一个地提交**

总共至少要有 **15-20 个 commit**，展现出你不断学习、不断进步的过程。

### 🚨 必须删除的文件 🚨

在你的新仓库中，**请务必删除以下文件**：

- `README.md` — 删掉
- `README-cn.md` — 删掉
- `TICKET.md`（如果有）— 删掉

这些是教程文件，不应出现在你的展示仓库中。

**只保留 `README.rst`** — 这才是一个正常的项目 README，看起来像是你自己探索学习后写的，而不是跟着教程抄的。你可以看看 `README.rst` 的内容，那就是一个很合格的项目说明。

### 最终效果

你的 public 仓库应该看起来像：
- 一个有十几到二十个 commit 的渐进式学习记录
- 只有 `README.rst` 作为项目说明
- 没有任何教程痕迹（没有 README.md、README-cn.md、TICKET.md）
- 向别人展现你是一个认真学习、持续进步的人
