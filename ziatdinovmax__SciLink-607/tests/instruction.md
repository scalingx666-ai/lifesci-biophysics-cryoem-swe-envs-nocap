# Issue

**Title:** fix: re-probe unavailable software on cache load so new installs are seen

## Description

# fix: re-probe unavailable software on cache load so new installs are seen

## Problem

The simulation agent reports an MLIP backend unavailable even after the user installs it. Live case: `pip install fairchem-core` succeeds, `import fairchem.core` works, yet the agent keeps saying UMA is unavailable.

## Root cause

`AvailableSoftware.auto()` loads the cached `~/.scilink/available_software.yaml` and returns it unchanged whenever the file exists — it never re-probes. A cache built before the install recorded `uma: available: false`, so the later install is invisible until a manual refresh. The live probe was correct (`fairchem.core` importable → `True`); only the stale cache was wrong. (The agent's "`pip install fairchem`" suggestion is a separate hallucination — the stored guidance text already says the correct `fairchem-core`.)

## Fix

On load, re-probe the entries that are **not yet available** and persist any that flipped to available (`_reprobe_unavailable`). This is cheap:

- A still-missing module's `importlib.util.find_spec` returns `None` without importing, so only a newly-installed package pays an import — once — after which it caches `available: true` and is skipped.
- Entries already `available: true` or `user_confirmed: true` are left untouched: trusted, and re-importing heavy MLIP packages on every startup is exactly the cost the cache exists to avoid. A stale *uninstall* surfaces as a clear import error at use time — cheaper than re-probing every installed backend on every startup.
- The re-probe is wrapped so a probe failure never breaks the load hot path.
- Also catches a newly-added skill bundle absent from an older cache, not just a newly-installed package.

## Verification

The stale cache self-healed on the next `auto()`: `uma` false → re-probed → `true` → persisted (YAML now records the `fairchem/core` origin). No manual `refresh`/delete needed.

## Tests

`tests/test_available_software_reprobe.py` — heal+persist, skipping trusted/user_confirmed entries, and still-missing staying false.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `9bdcd8571bb1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.