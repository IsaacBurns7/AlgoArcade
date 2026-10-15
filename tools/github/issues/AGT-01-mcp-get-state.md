---
id: AGT-01
title: MCP server with one tool on the fake backend
team: agent
owner: Jayden (Agent Engineer)
due: 2026-10-18
estimate: 2h
---
## Context
The language model plays through an MCP server that owns an emulator. This first version owns the ROM-free fake backend and answers one question: where is the player. AGT-02 adds the button tool and the project config next week.

## Deliverable
- `src/algoarcade/agent/server.py`: an `MCPServer` from the official `mcp` SDK v2, on stdio. Its lifespan creates one `FakeBackend`.
- One tool, `get_state`, that returns the `FakeState` fields as JSON.
- `uv run python -m algoarcade.agent.server` starts it.

## Acceptance criteria
- [ ] A test starts the server as a subprocess through the SDK's stdio client, calls `get_state`, and gets the same room, x and y as a fresh `FakeBackend` read through `FakeReader`.
- [ ] The same test lists the server's tools and finds exactly one, `get_state`.
- [ ] Nothing is printed to standard output except protocol messages. Logging goes to stderr.
- [ ] `uv run poe check` passes on your machine. The last lines are pasted in the pull request.
- [ ] The pull request is reviewed and merged into `main`.

## Notes
Contract: docs/harness-spec.md (MCP tool surface).

In SDK v2 the class is `MCPServer`, imported from `mcp.server.mcpserver`. The v1 path `mcp.server.fastmcp` no longer exists, so older examples will not import.

Out of scope: `press`, `.mcp.json`, FireRed (AGT-02 and AGT-03).
