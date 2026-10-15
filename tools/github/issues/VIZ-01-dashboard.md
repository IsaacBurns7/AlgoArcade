---
id: VIZ-01
title: Dashboard built, assembled and exported
team: pm
owner: Akshaj (Co-PM)
due: 2026-11-22
estimate: 6h
depends_on: OPS-03
labels: viz
---
## Context
Seven people build panels this week, so the page they drop into has to build by Tuesday. This item sets up the dashboard on fake-logger data first, then assembles the five panels on real runs and proves the static export works with no network. DOC-02 publishes what it produces.

## Deliverable
- By Tuesday Nov 17: a Quarto dashboard under `src/algoarcade/viz/dashboard/` that `uv run poe dashboard` renders to static HTML from fake-logger Parquet through DuckDB, with one Plotly figure and one embedded mp4, and a README on adding a panel.
- By Sunday: five pages from VIZ-02 to VIZ-06 on real runs, and a footer on every page with the git commit, the render date and the number of runs read.
- A walkthrough by both PMs, each on their own laptop with Wi-Fi off.

## Acceptance criteria
- [ ] On a fresh clone with only fake-logger data, `uv run poe dashboard` exits 0 and writes `index.html`. Done by Tuesday.
- [ ] A member who did not write the skeleton adds a panel by following the README, and the render still exits 0.
- [ ] With Wi-Fi off, the real-data site opens from disk, every figure responds to hover, every video plays, and the browser console shows no failed request.
- [ ] Both PMs comment here with one line per page: pass, or the defect. Each defect has its own issue in the W7 milestone.
- [ ] A panel not merged by Sunday is replaced by a static image of the matching W&B chart, labelled as such, and listed here.

## Notes
Contract: docs/technical-plan.md (Visualization).

`quarto-cli` downloads the Quarto binary at install. List every dependency this adds in the pull request. The repository is public, so GitHub Pages is available for DOC-02.

A W&B iframe needs the network, so a fallback must be an exported image. Plotly can load its JavaScript from a CDN; the console check catches that.

Out of scope: publishing (DOC-02), visual polish.
