# Delta Lake + Polars 入门教程

## 这门课学什么？

**Delta Lake** 是数据湖三驾马车之一，拥有 Python 和 Rust 客户端，性能极强。**Polars** 是新一代高性能 DataFrame 库，用 Rust 编写，速度远超 Pandas。两者结合，可以在不依赖 Spark 的前提下，用纯 Python 完成云端大数据湖的读写、合并、时间旅行、Schema 演进等核心操作。

这是**云计算时代大数据数据仓库的基础技能**。无论你将来做数据工程、数据分析还是机器学习，理解 Delta Lake + Polars 的工作方式都会让你受益匪浅。

## 学完这门课你会掌握

- **Delta Lake 核心操作** — 在 S3 上创建、读写 Delta 表，理解 overwrite / append / error 三种写入模式
- **Polars 数据处理** — 列操作、去重、聚合、Join、PII 脱敏等常见 ETL 手法
- **Merge / Upsert** — 增量更新数据的核心模式
- **时间旅行** — 按版本号回溯历史数据，查看 commit log
- **Vacuum 与 Schema Evolution** — 清理过期文件、安全地演进表结构
- **完整 ETL 管道** — 从 Bronze（原始数据）到 Silver（清洗后数据）的端到端流程


## 这个仓库里有什么

```
examples/
├── 00-minimal-poc/      # 最小可运行示例 + 背景概念
├── 01-delta-read-write/ # Delta Lake 基本读写（三种写入模式）
├── 02-polars-etl/       # Polars 数据处理（纯内存，不需要 AWS）
├── 03-merge-upsert/     # 合并/更新操作（Delta Lake 核心能力）
├── 04-time-travel/      # 时间旅行（版本回溯 + commit log）
├── 05-vacuum-schema/    # 清理旧文件 + Schema 演进
└── 06-full-etl/         # 完整的 Bronze→Silver 管道（需按顺序运行）
```

7 个模块，16 个 POC 脚本，从简单到复杂逐步推进。每个脚本都是独立的（`06-full-etl` 除外，需要按顺序跑）。所有数据都是 mock 的，不需要真实数据源。


## 环境准备

```bash
mise install       # 安装 Python 3.12 和 uv
mise run inst      # 创建虚拟环境，安装所有依赖
cp .env.example .env  # 复制环境变量模板
# 编辑 .env，填入你的 AWS_PROFILE 名称
```

验证环境：

```bash
python examples/00-minimal-poc/s01_minimal_poc.py
```

如果能跑通并输出一个 3 行的 DataFrame，说明环境配置成功。


## 如何学习

在 Claude Code 中依次使用以下命令：

| 顺序 | 命令 | 作用 |
|------|------|------|
| 1 | `/learn-this-project-absorb` | 从头到尾带你走一遍项目，逐个讲解每个组件的 WHAT 和 WHY |
| 2 | `/learn-this-project-quiz` | 小题快答，检验你是否真的理解了 |
| 3 | `/learn-this-project-elevate` | 看看高级工程师会如何改进这个项目，学习进阶方向 |
| 4 | `/learn-this-project-interview` | 模拟面试，压力测试你的理解深度 |
| 5 | `/learn-this-project-demo` | 练习如何向别人展示这个项目 |

学习过程中请务必**自己运行每个脚本**，并去 AWS Console 上看 S3 里到底发生了什么。


## 展示你的学习成果

学完之后，把你的学习过程展示在 GitHub 上：

1. **创建新的 public 仓库**，命名建议：`firstname-lastname-delta-lake-polars-poc`
2. **分步提交**（15-20 个 commit）：先配置文件，再核心包，再一个一个提交 examples
3. **删除教学文件**：`README.md`、`README-cn.md`、`TICKET.md`、`docs/learn-this-project/`、`.claude/skills/learn-this-project-*/`
4. **只保留 `README.rst`** 作为项目说明
5. **写你自己的 README**（可选）——用你自己的话描述这个项目
