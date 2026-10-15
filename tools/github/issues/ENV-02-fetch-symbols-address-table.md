---
id: ENV-02
title: Fetch the symbol files and start the FireRed address table
team: environment
owner: Prince (Memory Engineer)
due: 2026-10-18
estimate: 2h
labels: critical-path
---
## Context
Every FireRed address we use comes from the symbol files that the pokefirered decompilation publishes, never from a wiki. This step fetches them and records the first nine addresses with their source. ENV-05 builds the StateReader on this table next week.

## Deliverable
- `scripts/fetch_symbols.py`: fetches `pokefirered.sym` and `pokefirered_rev1.sym` at commit `3b401eaf14f193946899eb7c056d5ac02a470126` into `data/pokefirered/`.
- An address table in the FireRed folder under `src/algoarcade/games/`, keyed by ROM SHA-1, holding the nine symbols in docs/harness-spec.md (FireRed memory map). Each entry names its symbol and the commit.

## Acceptance criteria
- [ ] After `uv run python scripts/fetch_symbols.py`, both files exist under `data/pokefirered/` and `git status` shows nothing new.
- [ ] A test checks every table entry against the fetched files, matching on the base symbol name. It skips when the files are absent, and passes on your machine (output pasted).
- [ ] A ROM-free test parses three sample `.sym` lines written inline and gets the right name and address.
- [ ] `uv run python -m algoarcade.rom` on your FireRed dump prints rev 0 or rev 1. Output pasted.
- [ ] `uv run poe check` passes on your machine. The last lines are pasted in the pull request.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Contract: docs/harness-spec.md (ROM pinning, FireRed memory map).

File-local symbols carry a numeric suffix that changes between builds (`sGengarScroll.182` against `.185`). Match on the part before the dot.

The `.sym` files stay under `data/` and are never committed. If an address in the spec disagrees with the file, the file wins: fix the spec in the same pull request.

Out of scope: reading memory (ENV-05).
