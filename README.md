# Delta Lake + Polars on S3 — A Hands-On Repo

A small, deliberately-scoped codebase that teaches **Delta Lake + Polars on S3 — without Spark**. The patterns inside are extracted from a real data-modernization initiative at a financial organization whose production data lake supports incremental upserts, schema evolution, and audit-friendly time travel. You'll work through 16 self-contained proof-of-concept scripts in 7 progressive modules, then defend the design choices the way you would in an interview.

## What is this — the methodology in 30 seconds

This is a "learn-this-project" repo: a small codebase that teaches one vertical skill end-to-end. The point isn't to ship the code — it's to **absorb** the skill by running it, reading it, being able to defend every design choice, and finishing with a portfolio version on your own GitHub.

Six interactive skills make up the process:

- **`/learn-this-project-absorb`** — on-call mentor for the repo. Multi-mode: **Orient** (gives you the map plus a `files to READ` vs `files to RUN/DO` split), **Context-dive** (you bring a `file:line`, it unpacks that spot), **Next-step**, **Build** (helps you extend the repo with per-edit consent). It's a mentor, not a curriculum — use it when you need help, not as something to sit through linearly.
- **`/learn-this-project-quiz`** — discussion-style Q&A. Each answer is scored against the 3-part standard: **where to look + what + why**. A factually correct one-liner doesn't pass. Two modes: the pre-written bank (lower-bound check) and open-ended (you name a topic, it generates fresh questions).
- **`/learn-this-project-elevate`** — what's beyond this repo's current state. For each upgrade direction it walks current state → senior target → alternatives → prerequisite knowledge, then **converges into a concrete starter deliverable** you can hand back to Absorb Build mode to actually build.
- **`/learn-this-project-interview`** — full-project mock interview with pushback. Calibrates on role / format / time first, then runs five rounds (what you have / what you'd elevate / alternatives considered / problems hit / production pushbacks) and pushes back on every answer at least once.
- **`/learn-this-project-demo`** — script your live walk-through. The highest-value part is the cardinal-rule "don't show teaching artifacts" list — if the audience can tell this repo came from a tutorial, the demo flips from a positive signal to a negative one.
- **`/learn-this-project-publish`** — convert this teaching repo into a portfolio version on your own GitHub. Deletes teaching artifacts (with consent), generates a dependency-ordered commit cheat-sheet for you to copy-paste, co-writes your `README.md` in your own voice, finishes with a hostile-scan audit.

**Recommended order**: absorb → quiz → elevate → interview → demo → publish. Skills are mentors on call — invoke when you need orientation, context, or help. Not a curriculum to follow linearly.

## What's in this repo

```
.
├── examples/                      # 16 POC scripts, 7 progressive modules
│   ├── 00-minimal-poc/            # round-trip read/write/merge (golden reference)
│   ├── 01-delta-read-write/       # the 3 Delta write modes: overwrite, append, error
│   ├── 02-polars-etl/             # pure Polars in isolation (no S3 — start here)
│   ├── 03-merge-upsert/           # upsert with DeltaTable.merge(...)
│   ├── 04-time-travel/            # version reads + commit history
│   ├── 05-vacuum-schema/          # storage cleanup + schema evolution
│   └── 06-full-etl/               # Bronze→Silver capstone with incremental merge
├── delta_lake_and_polars_basic/   # core package
│   ├── one/                       # singleton built via mixin composition (boto3 + S3)
│   ├── paths.py                   # PathEnum — CWD-independent path resolution
│   ├── tests/, vendor/            # test plumbing + per-module pytest-cov helper
├── docs/learn-this-project/       # mentor's analysis (7 docs the skills read from)
├── mise.toml                      # task runner + tool versions (Python 3.12, uv)
├── pyproject.toml                 # dependencies (polars, deltalake, boto3, s3pathlib)
└── .env.example                   # AWS_PROFILE placeholder
```

Every example script imports the `one` singleton from the core package — that one object centralizes the AWS session, S3 root path, and the `polars_storage_options` dict every Delta call needs. Configure AWS once in `.env`, run any script.

## Tech stack and setup

- **Python 3.12**, managed by [mise](https://mise.jdx.dev/) (`mise.toml` pins the version)
- **[uv](https://docs.astral.sh/uv/)** for dependency resolution and venv management
- **[polars](https://pola.rs/) `>=1.39.3,<1.40.0`** + **[deltalake](https://pypi.org/project/deltalake/) `>=1.5.1,<1.6.0`** — Rust-native compute and table format, no JVM
- **boto3** + **boto-session-manager** + **s3pathlib** — AWS auth and S3 path ergonomics
- An AWS profile with read/write on a bucket matching `{account_alias}-{region}-data`

Bootstrap:

```bash
mise install                  # installs Python 3.12 + uv + claude + pandoc
mise run venv-create          # creates .venv (idempotent)
mise run inst                 # uv sync --all-extras
cp .env.example .env          # then edit: AWS_PROFILE="<your-profile>"
```

Smoke-test that `.env` loaded:

```bash
python -c "from delta_lake_and_polars_basic.one.api import one; print(one.s3_bucket)"
# Expected: prints '{account_alias}-{region}-data'
```

The runbook (`docs/learn-this-project/02-runbook.md`) has the full command table and the common-failures section.

## Recommended learning flow

The 6 skills, in the order most people get the most value from:

1. **`/learn-this-project-absorb`** (Orient mode) — start here. You'll get a 4–6 line summary, an architecture pass at the module level, then an explicit **files to READ** and **files to RUN/DO** split. Close the chat, work through the run-list yourself, come back in **Context-dive** mode whenever a specific spot needs unpacking.
2. **`/learn-this-project-quiz`** — verify the absorb stuck. Start with `random 10` from the bank; if you came up ⚠️ partial too often, drop into `module <name>` mode for the gaps, then re-quiz. Use **Open-ended mode** to drill anything you want harder.
3. **`/learn-this-project-elevate`** — once you can explain what's here, look at what isn't. Pick an area (tests / CI / prod-safety / observability / streaming / DX), walk the loop, and **converge a starter deliverable**. Hand it to Absorb Build mode if you actually want to ship it.
4. **`/learn-this-project-interview`** — pressure-test under conditions. Calibrate the role and format, then run all five rounds. Use the debrief's 3 weak-spot questions to decide what to re-absorb.
5. **`/learn-this-project-demo`** — script your live walk-through. The two beats most likely to land for this project are the **time-travel + commit-log** demo (`examples/04-time-travel/s02_history.py`) and the **incremental merge** capstone (`examples/06-full-etl/s03_incremental.py`).
6. **`/learn-this-project-publish`** — see the next section.

## Publish — turn this into a portfolio piece

When you can explain the project end-to-end, the next move is converting it into a clean public repo on your own GitHub. `/learn-this-project-publish` automates this end-to-end so you don't have to remember every step.

The cardinal rule: **the published repo must not be detectable as a teaching project.** If a hiring manager spots `README-cn.md`, `TICKET.md`, or `docs/learn-this-project/`, the signal flips from "this person learned a hard thing" to "this person ran a tutorial".

The skill walks you through:

- **Transform mode**: ask for your new repo name + your name, dry-run and delete cardinal teaching artifacts (with your consent every time), walk the borderline list, generate `tmp/publish-commit-plan.md` (a dependency-ordered cheat-sheet of 10+ commits in first-person past tense), then co-write your `README.md` section by section using *your own words*.
- **Audit mode**: hostile-scan the result with the question "did this come from a tutorial?" — reports findings as 🔴 HIGH / 🟡 MEDIUM / 🔵 LOW. Won't certify "ready to publish" until all 🔴 are cleared.

The skill never touches `git` itself — you do every `git add` / `commit` / `push` yourself from the cheat-sheet it generates. It also never creates the GitHub repo — that's your deliberate publication act.

## What mastery looks like

You can open any of the 16 scripts and explain what it's teaching and why it lives where it does in the curriculum. You can defend the Polars-vs-Spark choice in one sentence, name the demo-vs-prod flags (`AWS_S3_ALLOW_UNSAFE_RENAME=true`, `vacuum(retention_hours=0, ...)`) on sight, and walk a stranger through the Bronze→Silver merge with audit time-travel in under five minutes. When asked "what would you do with three more months?", you have a concrete starter deliverable, not a vague direction. And you have a clean portfolio repo on your own GitHub that survives a hostile scan.
