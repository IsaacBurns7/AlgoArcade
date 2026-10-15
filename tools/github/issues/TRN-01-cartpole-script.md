---
id: TRN-01
title: Train CartPole from a committed script
team: training
owner: Isaac (RL Engineer)
due: 2026-10-18
estimate: 2h
---
## Context
Before we train on FireRed we want one small, reproducible Stable-Baselines3 run that anyone can repeat. TRN-03 turns this script into the project's training runner next week.

## Deliverable
- `scripts/train_cartpole.py`: trains SB3 PPO on `CartPole-v1` from a `--seed` argument, passes `gamma` and every other PPO setting explicitly, writes the model under `data/runs/`, and prints the final mean evaluation return.

## Acceptance criteria
- [ ] `uv run python scripts/train_cartpole.py --seed 0` finishes and prints the evaluation return.
- [ ] Seeds 0, 1 and 2 each reach a mean evaluation return of at least 400 within 100,000 steps. A table of seed, steps, return and wall time is in the pull request, with one reward curve image.
- [ ] No PPO argument is left at its library default by omission: every one is written out in the script.
- [ ] `git status` after a run shows nothing under `data/`.
- [ ] `uv run poe check` passes on your machine. The last lines are pasted in the pull request.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Background: docs/technical-plan.md (Training).

Use `gamma=0.99` for CartPole, written out, and say why in a one-line comment: its episodes are short. On FireRed we never use 0.99.

The 400 bar is a guess at "clearly learning". If a seed misses, post the curve and what you tried.

Out of scope: config classes, run directories and checkpoints (TRN-03).
