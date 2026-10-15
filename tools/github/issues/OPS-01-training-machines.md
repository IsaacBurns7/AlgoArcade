---
id: OPS-01
title: Training machines ready
team: pm
owner: Sahil (lead PM)
due: 2026-10-18
estimate: 2h
labels: critical-path
---
## Context
Training runs on two machines: a Windows desktop (the box) and a Mac. OPS-02 benchmarks both next week and TRN-05 trains on them the week after.

## Deliverable
- Both machines with git, uv and a clone of the repo. The Mac also has `brew install mgba`; Windows needs no system package.
- Remote access to the box for members. Tailscale plus the OpenSSH server that ships with Windows is the suggested route.
- docs/setup.md (Training box): how to connect, where runs go, and the rule that nobody debugs on the box.
- A Weights and Biases Academic plan application sent from a tamu.edu address.

## Acceptance criteria
- [ ] `uv run poe check` passes on the box and on the Mac. Both outputs are pasted in the pull request.
- [ ] One member connects to the box remotely and runs `hostname`. Their output is pasted here.
- [ ] docs/setup.md (Training box) has the connection steps.
- [ ] docs/decisions.md records the date the Academic application was sent. Trackio is the fallback if it is not approved by Oct 28.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Background: docs/technical-plan.md (Simulation layer, Working practices).

PyPI's Windows torch wheel is CPU-only. Whether the box needs the CUDA build is decided in OPS-02, from the benchmark. Emulation is CPU-bound, so the default answer is no.

Out of scope: the benchmark (OPS-02), the run queue (OPS-04).
