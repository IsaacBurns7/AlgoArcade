---
id: ENV-01
title: Fetch the mGBA binding and import it on your machine
team: environment
owner: Rishi (Emulation Engineer)
due: 2026-10-18
estimate: 2h
labels: critical-path
---
## Context
FireRed runs on the prebuilt libmgba-py binding (release 0.2.0-2), and nobody on the team has installed it yet. This first step proves the download works on your own machine. ENV-04 builds the backend on top of it next week.

## Deliverable
- `scripts/fetch_mgba.py`: downloads the release archive for the host OS (win64, macos-arm64 or ubuntu-lunar), checks its SHA-256 against a value stored in the script, and unpacks the `mgba` package into the project environment.
- A test that runs without network.

## Acceptance criteria
- [ ] On your OS, `uv run python scripts/fetch_mgba.py` and then `uv run python -c "import mgba.core"` both exit 0. The output is pasted in the pull request.
- [ ] A test with a fake archive shows the script refusing a file whose SHA-256 differs from the stored value.
- [ ] Running the script a second time does not download again and exits 0.
- [ ] `uv run poe check` passes on your machine. The last lines are pasted in the pull request.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Contract: docs/setup.md (The mGBA binding).

Before running it: macOS needs `brew install mgba`, Ubuntu 24.04 needs `sudo apt install libmgba0.10t64`, Windows needs nothing. Compute each archive's SHA-256 once from the release page download and store all three.

Only your own OS has to pass this week. ENV-04 covers the other two.

Out of scope: the backend class and the conformance suite (ENV-04).
