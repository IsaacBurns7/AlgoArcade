---
id: VIZ-04
title: E1, E2 and E4 analysed, with comparison panels
team: training
owner: Isaac (RL Engineer)
due: 2026-11-22
estimate: 6h
depends_on: OPS-05, TRN-09, ENV-12
labels: viz, experiment
---
## Context
E1, E2 and E4 share one baseline arm on the S1-to-Pewter task. This item turns their runs into the numbers we report and the panels that show them, all through the OPS-05 statistics module. VIZ-01 assembles the page and DOC-05 quotes the numbers.

## Deliverable
- An analysis command that reads the Parquet runs and prints one table for E1, E2 and E4: IQM, 95% bootstrap interval, probability of improvement over the baseline, and seed count.
- `src/algoarcade/viz/comparison.py`, called once per experiment, drawing:
  - the IQM learning curve per arm with a 95% stratified bootstrap band, against environment steps;
  - the same against wall-clock hours, with the machine named on the axis;
  - a performance profile and the probability of improvement over the baseline.
- The seed count on every figure. An arm under five seeds is labelled exploratory.
- The best configuration and the reason, in docs/decisions.md.

## Acceptance criteria
- [ ] The analysis command reproduces the table from Parquet. Output pasted, with every seed's final score beside the aggregates.
- [ ] `uv run pytest tests/test_comparison.py` passes in CI on fake-logger data. One case puts arm A above arm B in every seed and asserts probability of improvement is 1.0.
- [ ] `grep -rnE "trim_mean|mannwhitneyu" src/algoarcade/viz/` prints nothing: the statistics come from the OPS-05 module.
- [ ] Every figure renders from the real runs with its seed count. Screenshots are in the pull request.
- [ ] The decisions entry states algorithm, observation mode and archive setting, with the numbers behind each.

## Notes
Contract: docs/technical-plan.md (Experiments, Visualization). Method: Agarwal et al. 2021.

With 5 seeds the IQM averages the middle three runs, and intervals on one task are wide. Do not call a difference real when the intervals overlap heavily. If some runs are unfinished on Nov 22, analyse what finished and state the count.

Fallback by Sunday: the W&B line chart of the same metric grouped by arm, exported as an image.

Out of scope: E3 and E5 (VIZ-05), the second game (VIZ-02), new runs.
