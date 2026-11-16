# Issue

**Title:** feat(flash): evaluate systems a domain at a time

## Description

Adds `so3lr.flash.domains.FlashDomains`, which evaluates energy and forces for systems larger than one GPU's memory, one domain at a time.

- The box is cut into domains with halos, and each force call makes three sweeps over them: per-atom values, long range, forces. The result is exact.
- Domains can be dealt over several GPUs of one node (`devices`).
- Long range is real space only; PME is refused.
- Force calls run at matmul precision `"highest"` by default.
- `FlashSO3LR` gets two hooks: a `w_e` weight in `_head_backward` and a `head` argument in `_backward_staged`.

Tests: `tests/test_flash.py` checks domains against one whole-system call (float64 for tauri-m and the legacy so3lr, float32, and two GPUs in both precisions), plus `pick_splits`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `67e03a087041`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.