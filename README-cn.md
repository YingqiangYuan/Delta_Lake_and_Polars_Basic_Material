# Delta Lake + Polars on S3 — 动手实战仓库

一个目标明确、规模克制的小项目：教你 **Delta Lake + Polars on S3（不用 Spark）** 的核心模式。素材抽自某金融机构数据现代化项目里真正在生产跑的数据湖架构 —— 它支持增量 upsert、schema 演化，以及为审计而生的 time travel。仓库里有 7 个递进模块、16 个独立可跑的 POC 脚本，你不仅要会跑、会读，最终还要能像在面试现场一样为每一处设计选择辩护。

## 这是什么 —— 30 秒方法论

这是一个 "learn-this-project" 项目：一个小型代码库，专门把某一项垂直技能从头讲到尾。重点不在交付代码，而在**吃透**这项技能 —— 跑得动、读得懂、每个设计决定都能解释清楚，最后还能在你自己的 GitHub 上发布一份干净的 portfolio 版本。

整个过程由 6 个交互式 skill 串起来：

- **`/learn-this-project-absorb`** —— 仓库的随身导师。多模式：**Orient**（给你整体地图，并明确划分**应该 READ 的文件**和**必须 RUN/DO 的文件**）、**Context-dive**（你给一个 `file:line`，它就地拆解那一处）、**Next-step**、**Build**（帮你扩展项目，每次编辑都要你点头）。它是导师，不是课程 —— 你需要帮助时才呼叫它，不是从头到尾跟着它走。
- **`/learn-this-project-quiz`** —— 讨论式 Q&A。每个回答按 3 段标准打分：**在哪里查 + 是什么 + 为什么**。事实正确但只有一句话的回答不算过。两种模式：写好的题库（保底校验）和开放生成（你指定话题，它现场生成新题）。
- **`/learn-this-project-elevate`** —— 这个仓库当前状态之外的事。对每个升级方向，它走一遍：当前状态 → senior 级目标 → 备选方案 → 前置知识，最后**收敛成一个具体的起步交付物**，你可以把这个交付物交给 Absorb Build 模式去真正动手实现。
- **`/learn-this-project-interview`** —— 完整的项目模拟面试，带反驳压力。先按角色 / 形式 / 时间做校准，然后跑 5 个 round（你有什么 / 你会升级什么 / 备选方案 / 遇到的问题 / 生产级 pushback），每个回答至少反驳一次。
- **`/learn-this-project-demo`** —— 帮你打磨现场 walk-through 的脚本。最有价值的部分是 "教学痕迹绝不能出现在屏幕上" 的红线清单 —— 一旦面试官看出这是 tutorial 项目，整场 demo 的信号就从正面翻成负面。
- **`/learn-this-project-publish`** —— 把这个教学仓库转成你自己 GitHub 上的 portfolio 版本。会在征得你同意后删除教学痕迹，生成一份按依赖顺序排好的 commit 速查表供你复制粘贴，按你自己的语气共同起草 `README.md`，最后用 "hostile-scan" 模式做一次审计。

**推荐顺序**：absorb → quiz → elevate → interview → demo → publish。这些 skill 是随叫随到的导师 —— 你需要找方向、需要 unpack 某个具体点、需要帮助时再呼叫它们。不要把它们当成一门要从头跟到尾的课程。

## 仓库里有什么

```
.
├── examples/                      # 16 个 POC 脚本，7 个递进模块
│   ├── 00-minimal-poc/            # 读-写-merge 一气呵成（黄金参考样本）
│   ├── 01-delta-read-write/       # Delta 的 3 种写入模式：overwrite / append / error
│   ├── 02-polars-etl/             # 纯 Polars，不接 S3（推荐先看这个）
│   ├── 03-merge-upsert/           # 用 DeltaTable.merge(...) 做 upsert
│   ├── 04-time-travel/            # 按版本读 + commit history
│   ├── 05-vacuum-schema/          # 存储清理 + schema 演化
│   └── 06-full-etl/               # Bronze→Silver 压轴，带增量 merge
├── delta_lake_and_polars_basic/   # 核心包
│   ├── one/                       # 通过 mixin 组合而成的 singleton（boto3 + S3）
│   ├── paths.py                   # PathEnum —— 不依赖当前工作目录的路径解析
│   ├── tests/, vendor/            # 测试脚手架 + 按模块隔离的 pytest-cov 工具
├── docs/learn-this-project/       # 导师视角的分析文档（6 个 skill 的资料来源）
├── mise.toml                      # 任务管理器 + 工具版本（Python 3.12、uv）
├── pyproject.toml                 # 依赖（polars、deltalake、boto3、s3pathlib）
└── .env.example                   # AWS_PROFILE 占位
```

每个例子脚本都从核心包导入 `one` singleton —— 这一个对象集中管好了 AWS session、S3 根路径、以及每次 Delta 调用都要传的 `polars_storage_options` 字典。在 `.env` 里配置 AWS 一次，之后任何脚本都能跑。

## 技术栈与初始化

- **Python 3.12**，由 [mise](https://mise.jdx.dev/) 管理版本（`mise.toml` 已经钉死）
- **[uv](https://docs.astral.sh/uv/)** —— 依赖解析和虚拟环境管理
- **[polars](https://pola.rs/) `>=1.39.3,<1.40.0`** + **[deltalake](https://pypi.org/project/deltalake/) `>=1.5.1,<1.6.0`** —— Rust 原生计算引擎和表格式，不需要 JVM
- **boto3** + **boto-session-manager** + **s3pathlib** —— AWS 鉴权和 S3 路径操作
- 一个有 S3 读写权限的 AWS profile，bucket 名要符合 `{account_alias}-{region}-data`

初始化：

```bash
mise install                  # 安装 Python 3.12 + uv + claude + pandoc
mise run venv-create          # 创建 .venv（幂等）
mise run inst                 # uv sync --all-extras
cp .env.example .env          # 编辑：AWS_PROFILE="<你的 profile>"
```

冒烟测试 `.env` 是否生效：

```bash
python -c "from delta_lake_and_polars_basic.one.api import one; print(one.s3_bucket)"
# 期望输出 '{account_alias}-{region}-data' 这种形式
```

完整命令表和常见故障排查参见 `docs/learn-this-project/02-runbook.md`。

## 推荐学习路径

6 个 skill，按多数人收益最大的顺序排列：

1. **`/learn-this-project-absorb`**（Orient 模式）—— 从这里开始。你会得到 4–6 行的项目概述、按模块走一遍架构，然后是一份明确的 **files to READ** 和 **files to RUN/DO** 清单。关掉对话，自己把 run-list 跑一遍，遇到需要拆解的具体点再回来用 **Context-dive** 模式。
2. **`/learn-this-project-quiz`** —— 验证你 absorb 到位没有。先用 `random 10` 从题库抽题；如果 ⚠️ partial 出现太多次，切到 `module <name>` 模式补漏洞，再 quiz 一次。需要进一步拷打的话，用 **Open-ended 模式** 自己点话题。
3. **`/learn-this-project-elevate`** —— 当你能解释清楚 "现在有什么" 之后，看看 "还差什么"。挑一个方向（tests / CI / 生产安全 / 可观测性 / 流式 / DX），走一遍循环，最后**收敛出一个具体的起步交付物**。想真的动手做，就把这个交付物丢给 Absorb Build 模式。
4. **`/learn-this-project-interview`** —— 在压力下检验。先校准角色和形式，然后跑完整 5 个 round。用 debrief 给的 3 个薄弱问题决定回去重新 absorb 哪部分。
5. **`/learn-this-project-demo`** —— 打磨你的现场 walk-through 脚本。这个项目里两段最容易打动观众的是 **time-travel + commit 日志** 这段（`examples/04-time-travel/s02_history.py`）和**增量 merge 压轴**（`examples/06-full-etl/s03_incremental.py`）。
6. **`/learn-this-project-publish`** —— 看下一节。

## Publish —— 转成 portfolio 作品

当你能从头到尾讲清楚这个项目时，下一步就是把它转成你自己 GitHub 上一份干净的公开仓库。`/learn-this-project-publish` 全程自动化，你不用记住每一步。

红线只有一条：**发布出去的仓库不能被看出是 tutorial 出身**。如果面试官看到 `README-cn.md`、`TICKET.md`、`docs/learn-this-project/`，信号会从 "这个人攻克了一项硬技能" 翻成 "这个人跑完了一套教程"。

这个 skill 会带你走：

- **Transform 模式**：先问你新 repo 名和你的名字，然后预演 + 删除教学痕迹（每一步都要你点头），过一遍 borderline 列表，生成 `tmp/publish-commit-plan.md`（按依赖顺序排好的 10+ commit 速查表，commit message 用第一人称过去时），最后用 *你自己的话* 一节一节地共同起草 `README.md`。
- **Audit 模式**：用 "这个仓库是不是 tutorial 出身？" 的视角做敌意扫描，把发现按 🔴 HIGH / 🟡 MEDIUM / 🔵 LOW 分级。所有 🔴 没清干净之前，不会判定 "可以发布"。

这个 skill 不会动 `git` —— 所有 `git add` / `commit` / `push` 都是你自己用它生成的速查表跑的。它也不会替你创建 GitHub repo —— 那一步是你主动决定发布的动作。

## 什么叫掌握

打开 16 个脚本里的任何一个，你能说清楚它在教什么、为什么放在课程里的这个位置。你能一句话辩护 Polars-不-Spark 的选择，看到 `AWS_S3_ALLOW_UNSAFE_RENAME=true`、`vacuum(retention_hours=0, ...)` 这类 "demo-only" 的旗帜会立刻识别出来，能在 5 分钟内把 Bronze→Silver merge 和 time-travel 审计这条线讲完。被问到 "再给你 3 个月你会做什么？" 时，你给得出一个具体的起步交付物，而不是一个含糊的方向。最后，你自己的 GitHub 上有一份能扛住敌意扫描的干净 portfolio 仓库。
