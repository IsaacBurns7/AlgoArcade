---
id: OPS-06
title: Queue the experiments, launch the Brock runs, freeze features
team: pm
owner: Sahil (lead PM)
due: 2026-11-15
estimate: 5h
depends_on: OPS-04, ENV-09, ENV-13
labels: critical-path, experiment
---
## Context
This week every experiment goes into the queue and the feature freeze lands on Nov 15. Two machines share about two weeks of compute before the results freeze on Nov 22, so run order decides what we can report. TRN-09, TRN-10 and ENV-12 submit their arms here. OPS-07 freezes results from what this item queues.

## Deliverable
- The shared baseline arm (PPO, symbolic, count, archive off) and the E4 archive-on arm, 5 seeds each, queued first because their code is ready first.
- The PQN, pixels, none, RND and E5 arms queued as their owners submit them.
- The compute budget table in docs/technical-plan.md (Experiments), rebuilt from the measured steps per second.
- E6 from S1: 3 seeds of the baseline config with the archive on, target the Brock badge. E6 from S0: 2 seeds, queued behind.
- The week's video review.
- The feature freeze checklist, filled in on this issue.

## Acceptance criteria
- [ ] The queue status, pasted here, shows 30 runs for E1 to E4 (6 arms, 5 seeds each) and 6 for E5, all on the S1-to-Pewter task with one step budget.
- [ ] The budget table names the run its steps per second came from, gives box-days and a finish date per experiment, and says what gets cut if an arm cannot finish by Nov 22.
- [ ] The queue lists 3 E6 runs from S1 ahead of 2 from S0. Success is graded by the badge flag `FLAG_BADGE01_GET` (0x820), evaluated with the archive off.
- [ ] Every finished run under data/runs/ has per-step logs and a checkpoint and passes the OPS-03 schema validation. The run count and zero failures are pasted.
- [ ] `gh pr list --state open` is pasted with each pull request marked fix, viz or deferred. None adds a feature.
- [ ] docs/decisions.md has an entry dated 2026-11-15 listing what was cut or deferred.

## Notes
Contract: docs/technical-plan.md (Experiments) and docs/development-plan.md (Week 5).

Arithmetic on the 500 steps/s planning figure, not a measurement: E1 to E4 at 15M steps a run take about 10 box-days, and the S1 Brock runs about 7. They do not both fit by Nov 22. The Brock runs keep running after the results freeze, and the writeup reports where they stood.

The Brock runs use the baseline config with the archive on, because the E1 to E4 results are not in yet. That is a choice, and the writeup says so.

Out of scope: analysing results (VIZ-04, VIZ-05), freezing them (OPS-07).
