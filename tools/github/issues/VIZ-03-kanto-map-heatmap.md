---
id: VIZ-03
title: Schematic Kanto map and the exploration heatmap
team: environment
owner: Prince (Memory Engineer)
due: 2026-11-22
estimate: 6h
depends_on: ENV-10, OPS-03
labels: viz
---
## Context
The heatmap is the first thing a visitor sees on the dashboard: where the agent went, and how that changed as it trained. pret/pokefirered has no license and its tiles are Nintendo art, so the base map is a schematic drawn from collision data. VIZ-01 assembles the page and DOC-02 publishes it.

## Deliverable
- `src/algoarcade/viz/kanto_map.py`: draws the 63 connected overworld maps in flat colours by behaviour class (walkable, wall, grass, water, ledge), placed with the ENV-10 world coordinates, plus inset panels for warp-only maps. A function maps (map_group, map_num, x, y) to a pixel.
- `src/algoarcade/viz/heatmap.py`: visits per (run, checkpoint window, map_group, map_num, x, y) from the visit-count table, on a log scale with a perceptually uniform colormap, one frame per window with a slider.
- A timeline of the first step each ENV-13 milestone was reached, one row per seed.
- A heatmap page in `src/algoarcade/viz/dashboard/`.

## Acceptance criteria
- [ ] `uv run python -m algoarcade.viz.kanto_map --out <path>` exits 0 and prints 63 placed maps. The render has insets for at least Viridian Forest (1,0) and Pewter Gym (6,2).
- [ ] A CI test on a synthetic map.bin decodes cell 0x1401 to metatile 1, collision 1, elevation 1.
- [ ] A CI test on a fake-logger run with a known walk gives the exact count for three named tiles.
- [ ] For one real baseline run, the counts sum to the run's total environment steps from the per-run table. Both numbers are pasted.
- [ ] Screenshots in the pull request show two slider frames, both insets and the timeline, with the page's HTML size.
- [ ] The pull request adds no image, Parquet or map.bin file.

## Notes
Contract: docs/harness-spec.md (Logging schema) and docs/technical-plan.md (Visualization).

Each map.bin cell is 16 bits: bits 0 to 9 metatile, 10 to 11 collision, 12 to 15 elevation. Read src/fieldmap.c in the decomp for the behaviour mask before drawing classes. Reuse the ENV-10 coordinate table; a second placement walk would drift.

Build on fake-logger data first, then switch the runs directory. Plotly embeds its data in every figure, so merge windows if a run has more than about 50.

Fallback if the global layout is not ready by Sunday: a grid of per-map heatmaps. Say which one shipped.

Out of scope: tile-art rendering, a live map stream.
