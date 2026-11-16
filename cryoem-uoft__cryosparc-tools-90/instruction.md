# Issue

**Title:** Numpy 2.0 support

## Description

- Looser numpy version requirement
- `__array_wrap__` deprecation fix
- Replace `np.array(a, copy=False)` with `np.asarray(a)`

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `81f193d0e46a`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.