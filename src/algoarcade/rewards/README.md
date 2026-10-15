# rewards

Reward components and the registry that combines them. `base.py` holds the protocol: a name, a reset step and a scoring step over two game states.

The first component, `explore`, arrives with TRN-02. The registry and four more follow in TRN-04 (Pri).

Contract: `docs/harness-spec.md`, Reward registry. Every component returns zero on a no-op step and on the reset step, and every pull request that adds one says how an agent would exploit it.
