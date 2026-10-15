---
id: TRN-02
title: Write the explore reward component
team: training
owner: Pri (Reward Engineer)
due: 2026-10-18
estimate: 2h
---
## Context
Every reward term is a small component behind the protocol in `src/algoarcade/rewards/base.py`. `explore` is the first one: it pays the first visit to a location in an episode. TRN-04 builds the registry and four more components on the pattern this sets.

## Deliverable
- `src/algoarcade/rewards/explore.py`: a component that pays 1.0 the first time `state.location` is seen in an episode and 0.0 otherwise. `reset` clears the visited set and marks the starting location as visited.
- Tests on the fake game (`FakeBackend` and `FakeReader`).

## Acceptance criteria
- [ ] Tests show: 0.0 on the reset step, 0.0 on a step where nothing moved, 1.0 on a new tile, 0.0 on a tile already visited.
- [ ] A test walks through the door into the second room and gets 1.0 there, because the room is part of the location.
- [ ] A test shows `reset` clears the episode: a tile visited before the reset pays again after it.
- [ ] The pull request says in two sentences how an agent could try to farm this component and why it cannot.
- [ ] `uv run poe check` passes on your machine. The last lines are pasted in the pull request.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Contract: docs/harness-spec.md (Reward registry).

The weight is not the component's job. It returns the raw value; the registry (TRN-04) applies weights.

Out of scope: the registry and the other components (TRN-04).
