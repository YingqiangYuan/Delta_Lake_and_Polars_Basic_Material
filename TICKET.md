# Delta Lake + Polars: Complete the POC Deep Dive

## Objective

Master the fundamentals of Delta Lake and Polars on S3 by running every example script, understanding the underlying concepts, and proving your knowledge through the Quiz.

Read the tutorial: [Delta Lake + Polars Hands-On Introduction](https://github.com/easyscale-academy/learn_delta_lake_and_polars_basic-project/tree/01-Learn-This-Project)

## Actionable Items

1. **Set up your environment** — Run `mise install && mise run inst`, configure `.env` with your AWS profile
2. **Run every script in `examples/`** — Execute each one individually, observe the output, and check S3 after each run to see what changed
3. **Read each module's README** — Every subfolder in `examples/` has a README.md explaining the concepts for that section
4. **Use `/learn-this-project` Guided Tour** — Let the AI walk you through the project structure and explain key patterns
5. **Take the Quiz** — Use `/learn-this-project` Quiz mode to test your understanding
6. **Iterate until 80%+** — For any question you can't answer clearly in plain language, go back to the relevant script and study it again
7. **Showcase on GitHub** — Create your own public repo with incremental commits (see README for details)

**Estimated time:** 4-8 hours (spread across multiple sessions)

## Checklist

- [ ] **Environment running** — `mise install && mise run inst` completes without errors, `.env` configured
- [ ] **All scripts executed** — Every script in `examples/00` through `examples/06` has been run successfully
- [ ] **S3 inspection done** — You've logged into AWS Console and observed the Delta table structure (Parquet files + `_delta_log/`)
- [ ] **Guided Tour completed** — You've gone through the full `/learn-this-project` Guided Tour at least once
- [ ] **Quiz passed at 80%+** — You can answer 80% of Quiz questions in plain English, as if explaining in a job interview
- [ ] **GitHub showcase created** — Public repo with 15-20 incremental commits, only `README.rst` as README, no tutorial files

## Grading Rubric

- **Environment:** Verify `.env` exists and `mise run inst` succeeds (check `uv.lock` or `.venv/` presence)
- **Script execution:** Ask the student to explain what any randomly chosen script does — they should be able to describe input, operation, and output without re-reading the code
- **Conceptual understanding:** The student can explain in plain language: (1) why Delta Lake uses a transaction log, (2) the difference between overwrite and append modes, (3) what merge/upsert does and when you'd use it, (4) what time travel enables, (5) why vacuum is necessary
- **Quiz benchmark:** Run 5 random Quiz questions — student should answer at least 4 clearly in conversational English, demonstrating understanding of "why" not just "what"
- **GitHub showcase:** Verify the public repo exists, has 15+ commits with progressive additions, contains no tutorial files (README.md, README-cn.md, TICKET.md), and only uses README.rst as the project description
