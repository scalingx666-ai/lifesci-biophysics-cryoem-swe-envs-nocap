# Issue

**Title:** Meta: checkpoints serialize a copy of the ledger

## Description

> Stacked on #679 (the checkpoint and ledger-lock work). Review after #679.

## Summary

While parallel analyses run, the meta agent saves its checkpoint at the same moment the branches update their entries in the delegation list. When the two collided, the save failed ("dictionary changed size during iteration") and that checkpoint was silently skipped. Checkpoints now save a private copy of the list, so a branch writing at the same moment can no longer make one fail.

## Technical description

**The defect.** Raised in the #679 review. `_auto_checkpoint` serialized `self._delegation_ledger` directly, with `json` and `indent=2`, which uses the pure-Python encoder. Fan-out branch threads write to their entries without the lock: `_started_at`, `timed_out`, `late_result` and others in `fanout.py`, and the whole-entry update in `_close_delegation`. A key inserted during the iteration raised `RuntimeError` and the checkpoint was skipped.

**The change.**
- **`_ledger_snapshot()`** copies each container with `dict(d)` / `list(x)`. That is one C call, which holds the GIL for its whole duration. It then recurses into those copies, never into the live objects, so no concurrent single-key write can interleave, and no retry is needed. `copy.deepcopy` was tried first and failed: it walks dicts in Python and collided with a busy writer 494 times out of 500.
- **Multi-field writes take the ledger lock** (`_fanout_lock`, an RLock since #679), so a checkpoint never catches an entry half-updated. These are `_close_delegation`'s `entry.update(...)` and `_delegate`'s `depth` / `data_path` / `metadata`.
- **`fanout.py`'s single-key writes are unchanged.** The snapshot makes them safe without locking twenty sites.

**Tests: `tests/test_ledger_checkpoint_concurrency.py`.**
- **Deterministic:** an object whose `str()` (called through `default=str`) writes a new key into the live entry mid-serialization. This fails on #679 and passes here.
- **A busy branch writer:** 20 checkpoints under a hot writer with large, bounded entries.
- **Source check:** the close path updates the entry inside the lock.

`tests/test_depth_channel.py` and `tests/test_ui_stop.py` build the meta with `__new__`; they gain the lock attribute the constructor creates.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`): a meta fan-out over a Raman and an XRD pattern of one sample, with two concurrent analysis branches and fusion. Checkpoints were written throughout, with 0 "Auto-checkpoint failed" warnings, and the session restored with the same ledger (indices 1–3).

- Full offline suite: no failures that `main` does not have, except `test_grain_twin_proxy.py::test_threshold_is_tunable`. That test is untouched here, passes 3 of 3 on its own on this branch, and failed the same way once on #686. It is flaky and depends on test order or timing.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6b3776bbd0ad`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.