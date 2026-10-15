---
id: OPS-03
title: Logging pipeline, fake logger and schema freeze
team: pm
owner: Sahil (lead PM)
due: 2026-11-01
estimate: 5h
depends_on: ENV-05
labels: critical-path
---
## Context
Every dashboard figure reads Parquet, and W&B is only the live view. The schema freezes on Nov 1 so that every later run stays comparable. TRN-05 writes through it this week. VIZ-01 to VIZ-06 build on the fake logger before real runs exist.

## Deliverable
In `src/algoarcade/logs/`:
- A schema module for three tables: per-step, per-episode, per-run (fields: docs/harness-spec.md, Logging schema).
- A Parquet writer (`pyarrow.parquet.ParquetWriter`) that rolls files under `data/runs/run_id=ID/`.
- The fake logger (no ROM needed): a seeded random walker over real FireRed map ids that sometimes earns a badge, in the exact schema.
- A comment here from each team listing the fields its week 6 visual needs.
- The tag `schema-v1` on Nov 1.

The rule: if the StateReader is not validated on real memory (ENV-08) by Nov 1, the freeze moves to Nov 8. We do not freeze an untested schema.

## Acceptance criteria
- [ ] `uv run pytest -k log_schema` passes in CI: a batch with a missing field, an extra field or a wrong type is rejected.
- [ ] A test writes 10,000 fake steps and reads every closed file while the writer is still open. Rows and schema match.
- [ ] The fake logger output passes schema validation. The same seed gives identical tables twice.
- [ ] Measured bytes per per-step row and the projected size of one 15M-step run are pasted in the pull request.
- [ ] Sign-off comments from environment, training and agent are here.
- [ ] `git tag -l schema-v1` prints the tag on Nov 1, or a comment here moves the freeze to Nov 8 and cites ENV-08.

## Notes
Contract: docs/harness-spec.md, sections Logging schema, Fake backend and fake logger.

Gotchas: an open Parquet file cannot be read, so rolled files are how we watch a live run. At 500 steps/s (unmeasured), one box-day is about 43M per-step rows.

Every reward component gets its own column. Collect the ten names from TRN-06 before freezing.

Out of scope: the run queue (OPS-04), the dashboard (VIZ-01), W&B setup (OPS-01).
