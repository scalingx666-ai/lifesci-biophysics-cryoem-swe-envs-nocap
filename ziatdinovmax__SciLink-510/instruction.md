# Issue

**Title:** Checkpoint the meta session at the end of every successful chat() turn

## Description

## Summary

A meta session driven from a script or notebook — one `chat()` call per process — could finish its work and still be unresumable: restoring the session later found the conversation but not the state needed to build on it. This PR makes every successful turn save the session checkpoint, so a scripted session is always restorable exactly as it ended.

.

Note on scope: the worst symptom reported in #364 (a restored session losing its entire delegation ledger) was largely addressed by 22776a60, which checkpoints after every completed delegation. What remained — and what this PR closes — is the issue's actual proposal: `chat()` itself never checkpointed on successful turn completion, so a turn's non-delegation state (meta mode, message count, and any turn that changes state without closing a delegation) was still lost for headless single-turn sessions.

---

**Technical details**

- `chat()` now calls `_auto_checkpoint()` after `_save_history()` on every successful turn and resets `last_checkpoint_message_count`, keeping the turn-start interval check as a mid-turn backstop.
- The per-turn save is quiet (`_auto_checkpoint(verbose=False)`) so the CLI doesn't print a checkpoint line after every message; the per-delegation and interval saves keep their prints.
- Offline regression (`tests/test_meta_turn_checkpoint.py`): a single stubbed turn writes `checkpoint.json` with the ledger and message count and restores in a fresh agent; the error path still emergency-checkpoints. Existing fan-out robustness suite passes 62/62.
- Live-validated on Bedrock (`bedrock/us.anthropic.claude-opus-4-8`): process 1 ran one programmatic `chat()` turn that delegated a two-peak curve fit (Voigt centers 4.198±0.002 / 6.812±0.005 vs synthetic truth 4.2 / 6.8, R²=0.994) and exited with no explicit save; process 2 restored with `restore_checkpoint=True` and saw the successful delegation in the ledger — the issue's acceptance scenario.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1bf34534b9ec`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.