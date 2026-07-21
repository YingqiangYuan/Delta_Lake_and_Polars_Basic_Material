# TICKET: Delta Lake + Polars on S3 — Learning Checklist

[Tutorial](https://github.com/easyscale-academy/learn_delta_lake_and_polars_basic-project/tree/01-Learn-This-Project/)

## Objective

Absorb the core patterns of a Spark-free, Lambda-shaped data lake on S3 — Delta Lake reads/writes, Polars-based ETL, merge/upsert, time travel, vacuum, schema evolution, and a Bronze→Silver capstone — to the point where you can defend every design choice and ship a clean portfolio version on your own GitHub.

## Checklist

### Setup
- [X] Clone the repo and switch to the `01-Learn-This-Project` branch
- [X] `mise install` (installs Python 3.12 + uv + claude + pandoc)
- [X] `mise run venv-create && mise run inst`
- [X] `cp .env.example .env` and set `AWS_PROFILE="<your-profile>"`
- [X] Smoke-test: `python -c "from delta_lake_and_polars_basic.one.api import one; print(one.s3_bucket)"` prints a bucket name
- [X] Confirm your AWS profile has S3 read/write on the bucket matching `{account_alias}-{region}-data`

### Absorb (learn the content)
- [X] Run `/learn-this-project-absorb` in **Orient mode** for the high-level map and the `files to READ` vs `files to RUN/DO` lists
- [X] Run every script on the run-list yourself, in the runbook's recommended order — `02-polars-etl/*` first (no S3), then `00-minimal-poc`, then `01 → 03 → 04 → 05 → 06`
- [X] Watch `examples/04-time-travel/s02_history.py` output until the Delta `_delta_log/` model clicks
- [X] Run `examples/06-full-etl/s03_incremental.py` end to end and trace how it composes work from modules `01`, `02`, `03`, and `04`
- [X] Come back to `/learn-this-project-absorb` in **Context-dive mode** whenever a specific spot needs unpacking (e.g. the merge predicate aliasing, the `polars_storage_options` frozen-credentials trick, the mixin composition in `one/one_01_main.py`)
- [X] Be able to explain WHY `02-polars-etl/` deliberately doesn't touch S3
- [X] Be able to explain WHY every script begins with `s3dir_example.delete(bsm=bsm)`
- [X] Be able to explain WHY `AWS_S3_ALLOW_UNSAFE_RENAME="true"` is set in `one/one_03_boto_ses.py` and when it would be wrong

### Quiz (verify understanding)
- [ ] Run `/learn-this-project-quiz` in **Bank mode** with `random 10` — clear the floor (no ⚠️ partial / ❌ wrong) against the 3-part standard (where + what + why)
- [ ] If anything came up shallow on the `one` singleton, run `module one`; if shallow on merge/time-travel/vacuum, run the matching `module <name>`
- [ ] Use **Open-ended mode** to drill 2–3 topics you want pressed harder (e.g. `generate 5 harder questions about the merge predicate aliases`, or `generate 5 questions comparing append vs merge`)
- [ ] If a topic keeps coming up partial, re-read the matching section in `docs/learn-this-project/01-knowhow-inventory.md` or the source file, then re-quiz

### Elevate (see what's beyond)
- [ ] Run `/learn-this-project-elevate` and explore 1–2 upgrade directions — strong candidates for this repo: `testing` (the highest-leverage item — plumbing is in, tests aren't), `prod-safety` (split the demo-vs-prod `polars_storage_options`), or `observability` (add `loguru` so merge-vs-overwrite cost is visible)
- [ ] **Converge each chosen direction into a concrete starter deliverable** with a file path and a success criterion — e.g. "add `tests/test_examples_02.py` that subprocess-runs the three `02-polars-etl/` scripts and asserts exit 0"
- [ ] (Optional, high-value) Hand the deliverable to `/learn-this-project-absorb` in **Build mode** and actually build the first iteration
- [ ] Note down "next small projects" the roadmap surfaced (e.g. a `07-streaming/` module, CI workflow, type checking)

### Interview (pressure-test yourself)
- [ ] Run `/learn-this-project-interview`, calibrate the role + format + time, complete a full mock session
- [ ] Survive at least one pushback per question on Rounds 1–3 (what you have / what you'd elevate / alternatives considered)
- [ ] Review the debrief; for the 3 weak-spot questions, return to quiz / absorb and re-cover the gap

### Demo (learn to present)
- [ ] Run `/learn-this-project-demo`, rehearse at least the 5-minute version
- [ ] Lock the 5-minute golden path: `examples/README.md` → `04-time-travel/s02_history.py` → `06-full-etl/s03_incremental.py` → close on the Polars-vs-Spark tradeoff
- [ ] Walk through the cardinal-rule "do NOT show" list — confirm you know how to keep `README.md`, `README-cn.md`, `TICKET.md`, `docs/learn-this-project/`, and the six sibling skills off the screen during the demo

### Mastery Gate
- [ ] You can answer ~70% of quiz questions to the 3-part standard (where + what + why), not just factually
- [ ] You can survive at least one pushback round per interview question
- [ ] You have a clear list of "what I'd study next" from the elevate session, including at least one concrete starter deliverable
- [ ] You can deliver the demo without notes, without exposing any cardinal artifact

### Publish (turn it into a portfolio artifact)
- [ ] Decide on a new public repo name (pattern: `<firstname>-<lastname>-delta-lake-polars-poc` or similar)
- [ ] Run `/learn-this-project-publish` in **Transform mode** — the skill walks you through:
  - [ ] Intake: new repo name + your name (or byline)
  - [ ] Delete cardinal teaching artifacts (skill does this with your consent — `README.md`, `README-cn.md`, `TICKET.md`, `docs/learn-this-project/`, the six sibling `.claude/skills/learn-this-project-*/` directories; keeps `learn-this-project-meta/`)
  - [ ] Borderline review (you decide on `.idea/`, `delta_lake_and_polars_basic.egg-info/`, `htmlcov/`, `dist/`, etc.)
  - [ ] Generate `tmp/publish-commit-plan.md` (your copy-paste cheat-sheet of ~20 dependency-ordered commits in first-person past tense)
  - [ ] Co-write your `README.md` in your own voice (D-mode — it asks, you answer, it drafts, you edit)
- [ ] Verify **Audit mode** returns 0 🔴 HIGH RISK findings before publishing
- [ ] Create the public GitHub repo yourself (skill won't do this)
- [ ] Open `tmp/publish-commit-plan.md` and run the commits one at a time
- [ ] `git remote add origin <github-url>` and `git push -u origin main`
