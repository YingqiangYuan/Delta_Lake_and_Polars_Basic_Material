# TICKET: Delta Lake + Polars on S3 — Learning Checklist

## Objective

Master the fundamentals of Delta Lake and Polars on AWS S3 by running every example, understanding the WHY behind each design choice, and proving your knowledge through quiz and mock interview.

Read [Tutorial](https://github.com/easyscale-academy/learn_delta_lake_and_polars_basic-project/tree/01-Learn-This-Project/)

## Checklist

### Setup
- [ ] Clone the repo and switch to the `01-Learn-This-Project` branch
- [ ] Run `mise install && mise run inst` to set up the environment
- [ ] Copy `.env.example` to `.env` and configure your AWS profile
- [ ] Verify `python examples/00-minimal-poc/s01_minimal_poc.py` runs successfully

### Absorb (learn the content)
- [ ] Run `/learn-this-project-absorb`, complete the full walkthrough
- [ ] Run each of the 16 example scripts yourself, observe output and check S3
- [ ] Understand why Delta Lake uses a transaction log (not just raw Parquet)
- [ ] Understand the difference between overwrite, append, and merge/upsert
- [ ] Understand the Bronze-to-Silver ETL pattern and why PII masking happens at the Silver layer

### Quiz (verify understanding)
- [ ] Run `/learn-this-project-quiz`, complete at least one full round
- [ ] Score 80%+ on a 10-question random round
- [ ] Review and re-study any topics where you scored poorly

### Elevate (see what's beyond)
- [ ] Run `/learn-this-project-elevate`, explore at least 1-2 upgrade areas
- [ ] Note down which directions interest you (CDC, data quality, concurrency, etc.)

### Interview (pressure-test yourself)
- [ ] Run `/learn-this-project-interview`, complete a full mock session
- [ ] Review the debrief, note which questions need more prep

### Demo (learn to present)
- [ ] Run `/learn-this-project-demo`, rehearse at least the 5-minute version
- [ ] Walk through the "do NOT show" checklist

### Mastery Gate
- [ ] You can explain **at least 70%** of the knowledge points from the quiz bank
- [ ] You can answer interview-style questions with a concept and direction (even if not perfect)
- [ ] You have a clear list of "what I'd study next" from the elevate session

### Show Your Work
- [ ] Create your own public repo (renamed, no "learn" prefix)
- [ ] Commit incrementally (15-20+ commits)
- [ ] Delete all teaching artifacts (`docs/learn-this-project/`, `.claude/skills/learn-this-project-*/`, `README.md`, `README-cn.md`, `TICKET.md`)
- [ ] Write your own README
