# Delta Lake + Polars: Hands-On Introduction

> Learn the foundation of modern cloud data warehousing — Delta Lake and Polars on S3, no Spark required.

## Overview

Delta Lake is one of the "big three" lakehouse formats. Polars is a blazing-fast DataFrame library written in Rust. Together, they let you build production-grade data lake pipelines in pure Python with performance that rivals — and often beats — Spark clusters for medium-scale workloads.

Think of it this way: if S3 is your filing cabinet in the cloud, Polars is the speed-reader that processes your documents, and Delta Lake is the librarian who keeps everything versioned, consistent, and auditable.

This course walks you through the fundamentals using small, self-contained proof-of-concept scripts. No real data, no complex infrastructure — just you, Python, and an S3 bucket.

## Learning Objectives

In a world where every company is becoming a data company, understanding how data moves from raw ingestion to clean, queryable tables is foundational. Whether you end up in data engineering, analytics, or ML, these patterns show up everywhere — from fintech transaction pipelines to e-commerce recommendation systems.

By the end of this exercise, you will:

1. Read and write Delta tables on S3 using overwrite, append, and error modes
2. Process data with Polars — column operations, deduplication, aggregation, joins, and PII masking
3. Perform merge/upsert operations to incrementally update tables
4. Use time travel to read historical versions and inspect commit logs
5. Run vacuum to clean up old files and evolve table schemas safely
6. Build a complete Bronze-to-Silver ETL pipeline end to end

## Prerequisites

- **AWS fundamentals** — You know how to create an AWS account and configure CLI credentials (`aws configure`)
- **Claude Code** — You're comfortable using Claude Code's slash commands (`/` commands) for AI-assisted learning
- **mise-en-place** — You know this is the fastest way to set up a reproducible dev environment on any machine

If any of these are unfamiliar, complete those prerequisite courses first.

---

## Key Concepts

### What Is Delta Lake?

Imagine you have a folder of Parquet files on S3. They're fast to read, but there's no version history, no way to undo a bad write, and no safe way for two processes to write simultaneously. Delta Lake adds a transaction log on top of those Parquet files — like Git for your data. Every write becomes a commit. You can roll back, read historical versions, and merge new data atomically.

### What Is Polars?

Polars is what you'd get if someone rebuilt Pandas from scratch in Rust, threw out all the legacy baggage, and optimized for modern hardware. It's a DataFrame library that processes data in columnar format, uses lazy evaluation for query optimization, and runs multi-threaded by default. For the workloads in this course, it's 10-50x faster than Pandas.

### How They Fit Together

- **S3** = your cloud storage (like a hard drive in the sky)
- **Polars** = your data processing engine (reads, transforms, writes DataFrames)
- **Delta Lake** = your transaction layer (versioning, ACID guarantees, merge/upsert)

You write data with Polars, store it as Delta tables on S3, and use Delta Lake's APIs for operations like merge, time travel, and vacuum.

---

## Exercises

This course is organized into seven progressive modules. Each module lives in its own folder under `examples/`, and **each folder contains a `README.md`** that serves as a mini-tutorial for that topic.

### Exercise 1: The Minimal POC

**Goal:** Understand the complete read-write-merge cycle in one script.

**What to do:**

1. Run `python examples/00-minimal-poc/s01_minimal_poc.py`
2. Read the code and the folder's README — understand each block (setup, write, read, upsert)
3. Log in to AWS Console and look at your S3 bucket — see what files were created

**What you'll notice:**

Delta Lake doesn't store one big file — it creates a folder with Parquet data files and a `_delta_log/` directory containing JSON commit entries.

> **Key insight:** Every Delta operation is a transaction recorded in the log. That's what makes time travel and rollback possible.

---

### Exercise 2: Delta Read/Write Modes

**Goal:** Understand the difference between overwrite, append, and error modes.

**What to do:**

1. Run each script in `examples/01-delta-read-write/` one at a time
2. After each script, check S3 to see how the table changed
3. Read the folder's README for context

---

### Exercise 3: Polars ETL

**Goal:** Master core data transformations in Polars (no S3 involved — pure in-memory).

**What to do:**

1. Work through `examples/02-polars-etl/` scripts in order
2. Pay attention to Polars-specific patterns: `with_columns`, `group_by().agg()`, `when/then/otherwise`

---

### Exercise 4: Merge and Upsert

**Goal:** Learn how to incrementally update a Delta table without rewriting everything.

**What to do:**

1. Start with the basic upsert in `examples/03-merge-upsert/s01_basic_upsert.py`
2. Then tackle the realistic 200-row credit profile merge

> **Key insight:** Merge is the bread-and-butter of production data pipelines. New data arrives, you match it against existing records, update what changed, insert what's new.

---

### Exercise 5: Time Travel

**Goal:** Read historical versions of your data and inspect the commit log.

**What to do:**

1. Run scripts in `examples/04-time-travel/`
2. Try reading different version numbers and see what the data looked like at each point

---

### Exercise 6: Vacuum and Schema Evolution

**Goal:** Understand table maintenance — cleaning old files and safely adding columns.

**What to do:**

1. Run `examples/05-vacuum-schema/s01_vacuum.py` — observe which files get removed
2. Run `examples/05-vacuum-schema/s02_schema_evolution.py` — see how old rows handle new columns (nulls)

---

### Exercise 7: Full ETL Pipeline

**Goal:** Put it all together — a complete Bronze-to-Silver pipeline.

**What to do:**

1. Run `examples/06-full-etl/` scripts **in order** (s01 → s02 → s03)
2. Each depends on the previous one
3. After each step, check S3 to see the Bronze and Silver tables evolve

---

## How to Learn

### AI-Guided Learning

Open Claude Code and type:

```
/learn-this-project
```

This command loads all course context into the AI. It offers two modes:

- **Guided Tour** — The AI walks you through the project step by step, explaining each script
- **Quiz** — The AI tests your understanding with concept questions tied to the scripts

### Recommended Workflow

Open **two terminal windows** side by side:

1. **Window 1** — Learning: run `/learn-this-project`, choose Guided Tour
2. **Window 2** — Practice: run `/learn-this-project`, choose Quiz

As you learn:

- **Run the code yourself** — don't just read it. Execute each script and observe the output.
- **Check S3** — log in to AWS Console, look at your bucket, see what happened after each script
- **Download and inspect** — grab files from S3. The data files are in **Parquet** format — they're not human-readable (they're columnar binary, optimized for machine speed). Ask the AI to help you write a quick script to read them if you're curious about the contents.

### How to Know You've Learned It

Learning is a cycle: study → test → study again. You're done when:

1. **Every script runs successfully** — you've executed each one and understand the output
2. **You can pass the Quiz at 80%+** — if someone asked you these questions in a job interview, you could explain the concepts clearly in plain English (no code memorization needed — just the "what" and "why")

If you can't answer a Quiz question, go back to the relevant script and study it again.

### Pacing

This is a single lesson, but it's packed with content. Take your time — there's no rush.

Ask lots of questions. When you hit something confusing:

- **Factual questions** ("What does this parameter do?" "What's this line doing?") → Ask the AI directly. It'll give you a straight answer.
- **Open-ended questions** ("Is this architecture a good choice?" "What are the trade-offs here?" "How would this work at scale?") → Write these down and ask your mentor.

Learning to distinguish between these two types of questions is itself a valuable professional skill.

---

## Reflection: What Did We Learn?

After completing all exercises, you've built a mental model of the modern data lake stack:

- **Storage** — S3 as cheap, durable, infinitely scalable object storage
- **Processing** — Polars for fast, expressive DataFrame operations
- **Transaction management** — Delta Lake for versioning, ACID writes, and merge
- **Pipeline architecture** — Bronze (raw) → Silver (clean) as the foundational pattern

These aren't toys — this is the same architecture used by companies processing billions of rows daily, just scaled down to learnable size.

---

## Mentor's Note

**Why this exercise matters:**

I built this course because I've seen too many people jump straight into "big data" tools — Spark clusters, Airflow DAGs, complex orchestrators — without understanding what's actually happening to the data underneath. That's like learning to drive on a highway before you know where the brake pedal is.

Delta Lake + Polars gives you the core mental model: data comes in, gets versioned, gets transformed, gets merged. Once you truly understand this cycle at a small scale, scaling up is just infrastructure. The concepts don't change.

**Key insights:**

- The "lakehouse" pattern (Delta Lake, Iceberg, Hudi) is replacing traditional data warehouses across the industry. Learning one teaches you the principles of all three.
- Polars isn't just "faster Pandas" — its lazy evaluation and expression system represent a fundamentally different (and better) way to think about data transformations.
- Time travel and merge/upsert aren't exotic features — they're table stakes for any production data pipeline. You'll use them constantly.

**Next steps:**

Once you're comfortable here, explore: partitioned tables, Z-ordering for query performance, concurrent writes, and connecting Delta tables to query engines like DuckDB or Trino. Each of these builds directly on what you've learned.

---

## Quick Reference

**Setup:**
```bash
mise install
mise run inst
cp .env.example .env   # then edit with your AWS profile
```

**Run a script:**
```bash
python examples/00-minimal-poc/s01_minimal_poc.py
```

**AI-guided learning:**
```
/learn-this-project
```

**Key files:**
- `examples/` — All POC scripts, organized by topic (00-06)
- `examples/*/README.md` — Mini-tutorial for each module
- `.env` — Your AWS profile configuration
- `mise.toml` — Tool versions and task definitions

---

## Showcase Your Learning

When you're done, showcase your progress on GitHub. This course repo is private, but you can create your own public version.

**How to do it:**

1. Create a new **public** repository. Name it something personal — e.g., `jane-doe-delta-lake-polars-poc`. Don't just copy the course repo name.

2. **Commit incrementally** (15-20 commits minimum). Don't dump everything in one commit. Add files progressively — config files first, then source code, then examples one by one. This shows a genuine learning journey, not a copy-paste job.

3. **Remove course materials:**
   - Delete `README.md` (this tutorial)
   - Delete `README-cn.md` (Chinese tutorial)
   - Delete `TICKET.md` (task card)
   - Keep only `README.rst` as your project description — it reads like a natural project README, not a homework assignment.

The result: a public repo that demonstrates steady, methodical learning to anyone who visits your GitHub profile.
