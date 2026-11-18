# Issue

**Title:** Bugfix: allow ion diffusion to be jitted

## Description

The assert within `get_all_parameters()` broke because this function gets `jit`ted. We now move the assert to `.set()`. This might cause issues if the user uses `data_set()`, but I also left a very vocal warning on the tutorial notebook.

JAX's `checkify` did not work out of the box.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `675aeb950b06`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.