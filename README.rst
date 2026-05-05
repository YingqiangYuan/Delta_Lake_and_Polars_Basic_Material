Welcome to ``delta_lake_and_polars_poc`` Documentation
==============================================================================

About
------------------------------------------------------------------------------
This project is a hands-on learning repository created while working on a
data modernization initiative at a financial organization. The production
system relies on a modern data lake architecture that supports incremental
upserts, schema evolution, and audit-friendly time travel — all built on
`Delta Lake <https://pypi.org/project/deltalake/>`_ and `Polars <https://pola.rs/>`_ (without Spark).

To deepen my understanding of how these libraries work together, I extracted
the core data-wrangling patterns from the project and turned them into a
collection of small, self-contained proof-of-concept scripts. Each script
isolates a single concept — reading/writing Delta tables on S3, Polars
transformations, merge/upsert, vacuum, schema evolution — and can be run
independently with no real data involved.

See `examples/README.md <examples/README.md>`_ for the full index of POC
scripts and what each one demonstrates.
