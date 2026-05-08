# Delta Lake + Polars: Hands-On Introduction

## What You'll Learn

**Delta Lake** is one of the three major lakehouse formats — it adds ACID transactions, versioning, and schema evolution on top of Parquet files stored in S3. **Polars** is a Rust-based DataFrame library that runs 10-50x faster than Pandas for typical ETL workloads.

Together, they let you build production-grade data lake pipelines in pure Python — no Spark, no JVM, no cluster required. This is a foundational skill for modern data engineering.

By the end of this course, you will:

- Read and write Delta tables on S3 using overwrite, append, and error modes
- Process data with Polars — column ops, dedup, aggregation, joins, PII masking
- Perform merge/upsert to incrementally update tables
- Use time travel to read historical versions and inspect commit logs
- Run vacuum and evolve table schemas safely
- Build a complete Bronze-to-Silver ETL pipeline end to end


## What's in This Repo

```
examples/
├── 00-minimal-poc/      # Golden reference — complete read-write-merge in one script
├── 01-delta-read-write/ # Three Delta write modes and their semantics
├── 02-polars-etl/       # Core Polars transformations (pure in-memory, no AWS needed)
├── 03-merge-upsert/     # Delta Lake merge/upsert at different scales
├── 04-time-travel/      # Version-based historical reads + commit log inspection
├── 05-vacuum-schema/    # Storage cleanup + safe schema evolution
└── 06-full-etl/         # End-to-end Bronze→Silver pipeline (run in order)
```

7 modules, 16 POC scripts, progressively building from simple to complex. All scripts are idempotent and use mock data — no real data sources needed.


## Setup

```bash
mise install           # Install Python 3.12 and uv
mise run inst          # Create venv, install all dependencies
cp .env.example .env   # Copy env template
# Edit .env — set your AWS_PROFILE name
```

Verify everything works:

```bash
python examples/00-minimal-poc/s01_minimal_poc.py
```

If it prints a 3-row DataFrame without errors, you're good to go.


## How to Learn

Use these commands in Claude Code, in order:

| Order | Command | Purpose |
|-------|---------|---------|
| 1 | `/learn-this-project-absorb` | Walk through every component — what it does and WHY |
| 2 | `/learn-this-project-quiz` | Quick-fire Q&A to verify you actually internalized it |
| 3 | `/learn-this-project-elevate` | What a senior engineer would improve, and how to learn those skills |
| 4 | `/learn-this-project-interview` | Mock interview with pushback — pressure-test your understanding |
| 5 | `/learn-this-project-demo` | Practice presenting this project to different audiences |

Run every script yourself and check your S3 bucket in the AWS Console after each one.


## Show Your Work

After completing the course, showcase your learning on GitHub:

1. **Create a new public repo** — suggested name: `firstname-lastname-delta-lake-polars-poc`
2. **Commit incrementally** (15-20 commits) — config files first, then core package, then examples one by one
3. **Remove teaching artifacts** — delete `README.md`, `README-cn.md`, `TICKET.md`, `docs/learn-this-project/`, `.claude/skills/learn-this-project-*/`
4. **Keep `README.rst`** as the project description
5. **Write your own README** (optional) — describe the project in your own words
